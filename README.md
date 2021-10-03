# What's here?

This is a *prototype* of a simple [ASP.NET Core 2.0](https://docs.microsoft.com/en-us/aspnet/core/getting-started) REST API web app that uses [SQL Server Management Objects (SMO)](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) under the covers to provide a RESTful interface for SQL Server running anywhere.

You can run this REST API web app on Linux, macOS, Windows or Docker and optionally use [environment variables](#environment-variables) to connect to a local or remote SQL Server instance, Azure SQL Database and Azure SQL Data Warehouse.

For fun, I've added ```GET``` REST API end-points to:

- navigate **Server** -> **Databases** -> **Tables** -> **Columns**
- generate ```CREATE DATABASE``` and ```CREATE TABLE``` T-SQL scripts
- view data in tables

Thanks to SMO, the REST API always returns up-to-date information when any schema or data changes occur in the database while the web app is running.

Currently, only the the ```GET``` verb is supported. In the future, I plan to add support for:

- other REST verbs (```PUT```, ```POST```, ```UPDATE``` and ```DELETE```)
- more database objects such as Views, Stored Procedures, Schemas and UDTs
- pagination for tables with a large number of rows

## Try it out!

The instructions below are for a MacBook. Modify as needed if you're using Linux or Windows.



## Run locally with .NET Core

### Step 1: Get the source code

Download and install .NET Core for your operating system: <https://www.microsoft.com/net/core>

> *TIP:* If you have ```Git``` installed then you can do ```git clone https://github.com/sanagama/mssql-restapi.git``` instead.

- Browse to <https://github.com/sanagama/mssql-restapi>
- Click ```Clone or Download``` then click ```Download ZIP```
- Save the ZIP file to your ```HOME``` directory as ```~/mssql-restapi.zip```
- Extract the zip file to your ```HOME``` directory ```~/mssql-restapi```

### Step 2: Run SQL Server 2017 in Docker

Launch a ```Terminal``` window and type the following commands to run SQL Server 2017 in Docker:

```
cd ~/mssql-restapi
cat ./scripts/1-docker-pull-mssql.sh
./scripts/1-docker-pull-mssql.sh
```

### Step 3: Restore sample databases

Type the following commands in the ```Terminal``` window to restore a couple of sample databases to SQL Server 2017 running in Docker:

```
cd ~/mssql-restapi
cat ./scripts/3-docker-create-db.sh
./scripts/3-docker-create-db.sh
```

### Step 4: Run the REST API web app locally

Type the following commands in the ```Terminal``` window to run the REST API web app locally:

```
cd ~/mssql-restapi
dotnet restore
dotnet build
dotnet run
```

### Step 5: Play with the REST API

- Launch your browser and navigate to <http://localhost:5000/api/mssql>
- Click on the various links in the JSON response to navigate databases, tables, columns and table data in the SQL instance and generate scripts.

## Environment variables

You can pass environment variables to the REST API web app to connect to a local or remote SQL Server instance, Azure SQL Database and Azure SQL Data Warehouse.

Environment variable | Description
--------------- | ------------
**MSSQL_HOST** | Fully qualified server name. Defaults to *127.0.0.1* if not specified.
**MSSQL_PORT** | SQL Server port. Defaults to *1433* if not specified.
**MSSQL_DATABASE** | Initial catalog for the connection. Defaults to *master* if not specified.
**MSSQL_USERNAME** | Username for SQL Server authentication. Defaults to *sa* if not specified.
**MSSQL_PASSWORD** | Password for SQL Server authentication. Defaults to *Yukon900* if not specified.

## Use with Azure SQL Database or Azure SQL Data Warehouse

You can connect to Azure SQL Database or Azure SQL Data Warehouse by passing your connection information in environment variables when starting the REST API web app.

>*TIP:* Follow instructions at [Configure a server-level firewall rule](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal#create-a-server-level-firewall-rule) to allow the computer running the REST API web app to connect to your Azure SQL Database or Azure SQL Data Warehouse.

>*TIP:* Change **\<server\>**, **\<username\>** and **\<password\>** in the example below as appropriate to connect to your Azure SQL Database or Azure SQL Data Warehouse.


```

Type the following commands in the ```Terminal``` window to run the REST API web app locally with .NET Core:
```
MSSQL_HOST="<server>.database.windows.net" \
MSSQL_PORT="1433" \
MSSQL_USERNAME="<username>" \
MSSQL_PASSWORD="<password>" \
dotnet run
```

## Motivation

My main motivation for creating this prototype was to try out the [SQL Server Management Objects (SMO)](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) APIs that recently became available on .NET Core 2.0. 

See [this Tweet](https://twitter.com/sqltoolsguy/status/916445930387152896) from [@sqltoolsguy](https://twitter.com/sqltoolsguy) for the announcement.

Developers and system administrators can finally use the nifty SMO APIs in .NET Core 2.0 client apps (like this prototype) or PowerShell cmdlets on Linux, macOS and Windows to programmatically manage SQL Server running anywhere, Azure SQL Database and Azure SQL Data Warehouse.

Take a look at the [SQL Server Management Objects (SMO) Programming Guide](https://docs.microsoft.com/en-us/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) for samples and API reference documentation.

Happy coding with SMO APIs and SQL Server running everywhere ;-)
