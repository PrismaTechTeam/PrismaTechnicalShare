# ExecuteSQL

##  GetDTDetails, GetDT

 - Return datatable if successful execute and return null if
   unsuccessful execute

**Validation for DataTable**
• Check if datatable no have data. (if necessary )

    DataTable MainTable = ExecuteSQL.GetDTDetails(@"SELECT * FROM ABC", BOMid);  
    if (MainTable.Rows.Count < 1)
    {
    // do something if no data in table
    }
 
• Check if datatable no have any column and data.   
 
    DataTable MainTable = ExecuteSQL.GetDTDetails(@"SELECT * FROM ABC", BOMid);  
    if (MainTable == null)
    {
    // do something if table is empty no data row no column
    }

##  GetSingledata

 - Return a single object if not null.
 - Return null if object is null.
 - Return null if unsuccessful execute.

**Validation for GetSingledata**
   

     decimal qty;
     object Quantity = ExecuteSQL.GetSingleData($"SELECT ProductionQty FROM JobOrder where Id = {JoId}");
     
     // if Quantity is null will return 0.00m for qty if not null will return converted decimal Quantity 
     qty = Quantity == null ? 0.00m : Convert.ToDecimal(Quantity);

# Data Row Validation and Convert
## Example Code for  Convert datatype for Data Row 
    DataTable  NewTolorences = ExecuteSQL.GetDT("query")
    
    // check if datatable is not null and row count is more than 0
    if (NewTolorences != null && NewTolorences.Rows.Count > 0)
    {
		   // loop each row in table
          foreach (DataRow row in NewTolorences.Rows)
          {
            decimal plus = row["Plus"] == null || row["Plus"] == DBNull.Value ? 0.00 : Convert.ToDecimal(row["Plus"]);
            decimal minus = row["Minus"] == null || row["Minus"] == DBNull.Value ? 0.00 : Convert.ToDecimal(row["Minus"]);
          }
     }

> For example, if row["Minus"] is null or DBNull.Value will return 0.00 but if row["Minus"] have value will convert it to Decimal

    if(row["Minus"] == null || row["Minus"] == DBNull.Value)
    {
    	 plus = 0.00;
    }
    else
    {
    	plus = Convert.ToDecimal(row["Minus"])
    }

> Optimize to 

    decimal plus = row["Minus"] == null || row["Minus"] == DBNull.Value ? 0.00 : Convert.ToDecimal(row["Minus"])

> Check row["Minus"] == null || row["Minus"] == DBNull.Value to prevent the error

 - row["Minus"] == null is the value for Minus is null
 - row["DBNull.Value"] is the row is not exist mean empty data

# Convert

Use `Convert.ToDecimal(row["Minus"])`  instead of `(Decimal)row["Minus"]` for numeric datatype to prevent error.
For example, Numeric datatype

    int Less= Convert.ToInt32(row["Less"]);
    Double Add = Convert.ToDouble(row["Add"]);
    decimal Minux= Convert.ToDecimal(row["Minux"]);
    
But If `row["Name"]` datatype is string so we can use `(string)row["Name"]` to convert the string because string is nullable.

### BigInt type convert to Int64

For example: 
If `row["Id"]` datatype in database table is `bigint`, using Convert.ToInt64(row["Id"]) to convert it to int and make sure using Int64 datatype in C# 

    Int64 PartId = Convert.ToInt64(row["Id"]);

If Convert `bigint` in to `Int32/Int16` will occur error.
