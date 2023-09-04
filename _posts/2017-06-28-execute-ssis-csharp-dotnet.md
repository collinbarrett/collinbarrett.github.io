---
title: 'Executing an SSIS Package from .NET'
date: '2017-06-28T06:41:32-05:00'
author: 'Collin M. Barrett'
excerpt: 'Two methods for executing an SSIS package installed on a SQL Server instance from a C#/.NET application.'
layout: post-wp-import
permalink: /execute-ssis-csharp-dotnet
redirect_from: /execute-ssis-csharp-dotnet/
image: /assets/img/ssis_collinmbarrett.jpg
categories:
- Code
tags:
- Database
- Dotnet
- 'fred''s'
- 'Visual Studio'
---

The retail pricing application that I have been developing had a requirement to allow the user to trigger a process
implemented in SQL Server Integration Services. We added a button in the Kendo UI front-end, but wiring up the button to
trigger an SSIS package installed on a SQL Server instance was new for me. Here is how we did it.

## Executing SSIS Package Directly

A colleague and I spent a day or so trying to execute the SSIS package directly. It took so long because there had to be
communication with the DBA to get service accounts, access permissions, and package locations/names all configured
correctly. However, in the end, we found a working solution with one caveat.

### Dependencies

We needed first to ensure that we included a couple of dependencies.

```
using System.Data.SqlClient;
using Microsoft.SqlServer.Management.IntegrationServices;
```

### Execution

The following snippet shows the final working solution we came up with that triggers a package with a single parameter. The value of ObjectType in the parameter configuration refers to which of the following types of parameters it is: system (50), package (30), or project (20). The connection string we used was a new SQL Server service account created just for this purpose.

```
private void ExecuteSsisPackage()
{
using (var ssisSqlConnection = new SqlConnection(ConfigurationManager
.ConnectionStrings["SsisConn"]
.ConnectionString))
{
var ssisServer = new IntegrationServices(ssisSqlConnection);
var ssisPackage = ssisServer.Catalogs["SSISDB"]
.Folders["MyProject"]
.Projects["MyProject"]
.Packages["MyPackage.dtsx"];
var executionParameter = new Collection&lt;PackageInfo.ExecutionValueParameterSet&gt;
{
new PackageInfo.ExecutionValueParameterSet
{
ObjectType = 20,
ParameterName = "MyPackageParameter1Name",
ParameterValue = "MyPackageParameter1Value"
}
};
ssisPackage.Execute(false, null, executionParameter);
}
}
```

### Caveat

After iteratively testing through a variety of errors to get the solution above, we were then faced with one final error:

<figure aria-describedby="caption-attachment-4400" class="wp-caption aligncenter" id="attachment_4400" style="width: 790px">[![SSIS Execution Error Using SQL Server Authentication](/assets/img/SsisSqlServerAuthenticationError_collinmbarrett.jpg)](/assets/img/SsisSqlServerAuthenticationError_collinmbarrett.jpg)<figcaption class="wp-caption-text" id="caption-attachment-4400">SSIS Execution Error Using SQL Server Authentication</figcaption></figure>

Oh, the new SQL Server service account we had created could not trigger the package directly. I wish we had discovered that sooner. Using a Windows account was not an option in our environment, so we were forced to scrap this code and move on to plan B.

## Executing via SQL Job

SQL jobs, unlike SSIS packages, can, in fact, be triggered by SQL Server user accounts. So, we configured a SQL Job to perform the execution for us. SQL Server ships with a system stored procedure designed for just this purpose. So, the snippet below is fairly straight-forward and worked like a champ once we ensured that the job had the proper parameter value configured and that the SQL Server service account had permissions to execute this system stored procedure.

```
private void ExecuteSsisPackage()
{
using (var ssisSqlConnection = new SqlConnection(ConfigurationManager
.ConnectionStrings["SsisConn"]
.ConnectionString))
{
using (var execSsisStoredProcedure = new SqlCommand("dbo.sp_start_job", ssisSqlConnection))
{
execSsisStoredProcedure.CommandType = CommandType.StoredProcedure;
execSsisStoredProcedure.Parameters.AddWithValue("@job_name", "MyPackageSqlJob");
ssisSqlConnection.Open();
execSsisStoredProcedure.ExecuteNonQuery();
}
}
}
```

Note that for this method to work, we needed to properly set the catalog where the system stored procedure was located (MSDB). We did this in the connection string in the web.config like below, but there may also be a way to set this in the code snippet above.

```
&lt;connectionStrings&gt;
&lt;add name="SsisConn" connectionString="Data Source=myserver;Initial Catalog=MSDB;MultipleActiveResultSets=True;Persist Security Info=True;User ID=MySsisUser;Password=MySsisPassword" providerName="System.Data.SqlClient" /&gt;
&lt;/connectionStrings&gt;
```

*via [Microsoft TechNet](https://social.technet.microsoft.com/wiki/contents/articles/21978.execute-ssis-2012-package-with-parameters-via-net.aspx)*