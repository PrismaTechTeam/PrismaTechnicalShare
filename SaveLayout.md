# Grid View Save Layout

## Step 1: Import GridViewLayoutManager.cs in your project

    using DevExpress.XtraGrid.Views.Grid;
	using System;
	using System.IO;
	using System.Windows.Forms;

	namespace YourNameSpace.MyString.Helper.GridViewLayoutControl
	{
	    public class GridViewLayoutManager
	    {
	        private const string LAYOUT_FILE_EXTENSION = ".xml";
	        private const string folderName = "Layout";
	        private static readonly string appFolderPath = Application.StartupPath;


        /* Save the layout of the gridview to xml file anc create the folder if not exist */
        public static void SaveLayout(string FormName, GridView gridView)
        {
            string folderPath = Path.Combine(appFolderPath, folderName);
            if (CreateFolder(folderPath))
            {
                string filePath = Path.Combine(folderPath, $"{FormName}_{gridView.Name}{LAYOUT_FILE_EXTENSION}");
                gridView.SaveLayoutToXml(filePath);
            }
        }


        /* Create the layout folder if not exist */
        private static bool CreateFolder(string folderPath)
        {
            if (!Directory.Exists(folderPath))
            {
                try
                {
                    Directory.CreateDirectory(folderPath);
                    return true;
                }
                catch (Exception e)
                {
                    MessageBox.Show(e.Message);
                    return false;
                }
            }
            return true;
        }


        /* Load layout from file if existt */
        public static void RestoreLayout(string FormName, GridView gridView)
        {
            try
            {
                string folderPath = Path.Combine(appFolderPath, folderName);
                string filePath = Path.Combine(folderPath, $"{FormName}_{gridView.Name}{LAYOUT_FILE_EXTENSION}");
                gridView.RestoreLayoutFromXml(filePath);
            }
            catch
            {

            }
        }
 
    }
	}


## Step 2 : Custom Menu bar add "Save Layout' Button in your Form cs file & 	  Save Layout click event

        GridView RightClickOnGridView;
        private void gridView1_PopupMenuShowing(object sender, PopupMenuShowingEventArgs e)
        {
            // Check if the right-click was on a column header
            if (e.MenuType == DevExpress.XtraGrid.Views.Grid.GridMenuType.Column)
            {
                // Create a new menu item
                DXMenuItem customMenuItem = new DXMenuItem("Save Layout", OnCustomMenuItemClick);
                RightClickOnGridView = e.Menu.View;
                // Add the menu item to the context menu
                e.Menu.Items.Add(customMenuItem);
            }
        }

        private void OnCustomMenuItemClick(object sender, EventArgs e)
        {
            GridViewLayoutManager.SaveLayout(this.Name, RightClickOnGridView);
        }
	
After Copy paste this code to your form cs file, go to designer > properties > choose gridiview > Event (yellow thunder icon) > find `PopupMenuShowing` > drop down choose `gridView_PopupMenuShowing`
 
## Step 4: Load Layout 
      private void LoadData()
      {
      .
      .
       gridControl1.DataSource = ExecuteSQL.GetDTDetails(query2, $"{txtEmployeeID.EditValue}");
       GridViewLayoutManager.RestoreLayout(this.Name, gridView1);
      }
  Put `GridViewLayoutManager.RestoreLayout(this.Name, gridView1);` under where you load the data source for gridview/gridcontrol. 

    RestoreLayout(this.Name, PutYourGridView); 
   Change `PutYourGridView` to whatever gridview that you want

## How to save
Right Click on any column on the gridView and click Save Layout

## What next can improve?
1) Allow user restore to default layout and the saved layout will be delete
2) In Form cs should using less code to apply the save layout function
3) Need Prevent Duplicate Layout file name (.xml)
```cs
    using DevExpress.XtraGrid.Views.Grid;
```
