## Audit Log SOP

1. Import the necessary module:
    
    ```csharp
    using Prisma.Auth.Audit;
    ```
    
    
2. Add the following line of code during the save process:
    
    ```csharp
    AuditLog.AddAuditLog("String TableName", "String PrimaryKeyName", "int PrimaryKey", "int userid", "DateTime datetime", "string Message = "" ");
    ```
    
    
    Example:
    
    ```csharp
    AuditLog.AddAuditLog("Type", "Id", Id, 1, DateTime.Now, "Insert New Record");
    ```
    
    
    This logs an audit entry for the specified table, primary key, user ID, timestamp, and optional message during the save process. Adjust the parameters accordingly for each use case.
