# How to use UseReportTool for single dataset to create a new report 
![](https://i.ibb.co/zfxHRzN/image.png)

- Require Using 
```csharp
using static MES.Report.ReportStorageMain;
```

- Create a button name it as BtnPreview and enable click event 


```csharp
 private void BtnPreview_Click(object sender, EventArgs e)
 {
     if (gridView1.RowCount < 0)
     {
         AppMessage.ShowWarningMessage("No Data selected");
         return;
     }
     // get row data from first row in grid view  1
     DataRow dataRow = this.gridView1.GetDataRow(this.gridView1.FocusedRowHandle);
     if (dataRow != null)
     {
         string RecID = dataRow[0].ToString();
         // Step 1 : get data from database and assign as a DataTable 
         DataTable Data = executeSQL.GetDTDetails(mydb, PyrlSQL.PyrlSQLType.GetEmployeeData, RecID);
         // Step 2 : Name your DataTable (This name will present on your report designer datasource section)
         Data.TableName = "Employee Info";
         // Repeat Step : if you require more data can repeat the step
         DataTable Data2 = executeSQL.GetDT(mydb, PyrlSQL.PyrlSQLType.GetCompanyGeneralDT);
         Data2.TableName = "Company Info";
			
		// Open Preview Form and assign report type and merge the Data From DataTable to DataSet
         UseReportTool("EmployeePackage", FillDataset("EmployeePackage", Data, Data2));
     }
     else
     {
         AppMessage.ShowWarningMessage("No Data selected");
     }
 }
```
**Step 1:** Get data from database and assign as a DataTable 
```csharp
DataTable Data = executeSQL.GetDTDetails(mydb, PyrlSQL.PyrlSQLType.GetEmployeeData, RecID);
 ```

**Step2**: Name your DataTable (This name will present on your report designer Field List)
```csharp
Data.TableName =  "Employee Info";
 ```

**Note**: If you require more data can repeat the step 1 & 2
```csharp
DataTable Data2 = executeSQL.GetDT(mydb, PyrlSQL.PyrlSQLType.GetCompanyGeneralDT);
Data2.TableName = "Company Info";
 ```

**Step 3** : Open Preview Form and assign report type and merge the Data From DataTable to DataSet
```csharp
UseReportTool("EmployeePackage",  FillDataset("EmployeePackage", Data, Data2));
 ```

**Note**: `FillDataset` function 
 - Function `UseReportTool` require two parameter `UseReportTool(Report Type, Dataset)` 
 - In Preview form only can view all report with type "EmployeePackage" if you put `UseReportTool("EmployeePackage", ...` . 
 - Your new report type also under type "EmployeePackage".
 - `FillDataset(Report Type, Data, Data2))` require report type in the first parameter and all the datatable put behind on first. 
 - `FillDataset` will merge all the datatable and return a dataset.

**Step 4**: Click New Design
**Step 5**:  DataSource will present in Field List

![enter image description here](https://i.ibb.co/bb9N0SF/image.png)

**Step 6**: Custom your report
**Step 7**: File > Save As (For first time save)
**Note** : After save your report will store in your bin debug folder and filename is `ReportStorage.xml`

 - Right Click `ReportStorage.xml` and open with browser or text file to view the details.
 - `Ctrl + F` to find your Report using report name of report type in your `ReportStorage.xml`
 - After save, when you close and reopen the previes form will show your report on the gridview and allow you to modify the report.

# How to use UseReportToolSet for multiple dataset to preview multiple page report

- Require Using 
```csharp
using static MES.Report.ReportStorageMain;
```

- Create a button name it as BtnPreview and enable click event 

```csharp
 private void BtnPreview_Click(object sender, EventArgs e)
 {
     if (PaySlip() != null) {
         UseReportToolSet("Payroll Report", PaySlip(), "SimplePaySlip");
     }
 }
```
**Note**
 `UseReportToolSet`  require 3 parameter 
 - UseReportToolSet(*Report Type*, *Array Dataset*, *ReportURL/ReportName*);

**Note**： PaySlip() is a function will return a array Dataset 
 ```csharp
public DataSet[] PaySlip(){
   if (gridView1.RowCount < 0)
   {
       AppMessage.ShowWarningMessage("No Data selected");
       return null;
   }
   DataRow dataRow = this.gridView1.GetDataRow(this.gridView1.FocusedRowHandle);
   if (dataRow != null)
   {
       DataTable ListofEmployeeID = LoadEmployee(Int32.Parse(dataRow[2].ToString()), Int32.Parse(dataRow[3].ToString()));
       
       // declare and assign size for datasets array
       DataSet[] dataSets = new DataSet[ListofEmployeeID.Rows.Count];
	
       for (int i = 0; i < ListofEmployeeID.Rows.Count; ++i)
       {
       	   // Custom own datatable for report datasouc
           int EmpID = Int32.Parse(ListofEmployeeID.Rows[i]["ID"].ToString());
           DataTable Data = executeSQL.GetDTDetails(mydb, PyrlSQL.PyrlSQLType.GetEmployeeData, EmpID);
           Data.TableName = "Employee Info";
           DataTable Data2 = executeSQL.GetDT(mydb, PyrlSQL.PyrlSQLType.GetCompanyGeneralDT);
           Data2.TableName = "Company Info";
           DataTable Data4 = new DataTable();
           Data4.Columns.Add("ProcessYearMonth", typeof(string));
           Data4.Columns.Add("Date", typeof(string));
           Data4.Rows.Add(getFullName(Int32.Parse(dataRow[3].ToString())) + $" {Int32.Parse(dataRow[2].ToString())}", DateTime.Now.ToString("dd-MM-yyyy"));
           Data4.TableName = "Payroll Basic Info";
           DataTable Data3 = executeSQL.GetDTDetails(mydb, PyrlSQL.PyrlSQLType.PayrollReportByEmployee, Int32.Parse(dataRow[2].ToString()), Int32.Parse(dataRow[3].ToString()), EmpID);
           Data3.TableName = "Payroll";
	   
           // Add a dataset in dataSets array (Important)
           dataSets[i] = FillDataset("EmployeePackage", Data, Data2, Data3, Data4);
       }
       return dataSets;
   }
   else
   {
       AppMessage.ShowWarningMessage("No Data selected");
       return null;
   }
}
```

**Before use UseReportToolSet**: 

 - You need to use  `UseReportTool` for single dataset to create a new report first. 
 - Your dataset in `UseReportTool` and `UseReportToolSet` must be same but just in `UseReportToolSet` .
 - You need to customize your own Dataset array and Dataset.
 
