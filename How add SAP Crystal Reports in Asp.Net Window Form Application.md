# How to Integrate Crystal Report in Asp.net Window Form Application. 
# What are SAP Crystal Reports?
SAP Crystal Reports can help you analyze your data by creating richly formatted, pixel-perfect, and multipage reports from virtually any data source, delivered in over a dozen formats.
# Problem
To create reports in c#, we have to write a lot of code. We can do the same work quickly with Crystal Reports.

# Environment
Visual Studio, Asp.Net MVC, Window Form Application

# How you fix it
By following the steps given in the solution below:

# Solution 
You can use Crystal Reports in your Asp.net application by following these steps:
* First, you need to download and install  SAP Crystal Reports from the Official website: https://www.sap.com/cmp/td/sap-crystal-reports-visual-studio-trial.html 
* After installing the crystal reports, you have to add the crystal reports by selecting the new item and then Crystal Reports in your project.
* Design the crystal report according to your requirements.
* Now, Add Crystal Reports Viewer from the toolbar in your application.
* After adding the Repots Viewer, provide data source to crystal report.* Then add a reference of your application in the Unit test project.
Suppose the crystal report we added is Backuplistofcheques. Then, the following code will populate the crystal reports.
```
        Backuplistofcheques RC = new Backuplistofcheques();
            try
            {

                DataTable dt = dal.DalGenerateReportBackup(beneAccount, cheque, chequenumber, duedate);
                RC.SetDataSource(dt);
                crystalReportViewer1.ReportSource = RC;
                crystalReportViewer1.Refresh();

            }
            Catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }

```
* Here, DalGenerateReportBackup is the method which returns data from the database as a data table.
