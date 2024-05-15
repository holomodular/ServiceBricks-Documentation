# Supported Database Engines
## Overview
The ServiceBricks platform supports several different relational (SQL) and document (NoSQL) database engines.

### Azure Data Tables
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/azuredatatables.svg" alt="azuredatatables logo" width="100"></img>

### Azure SQL
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/azuresql.svg" alt="azuresql logo" width="100"></img>

### CosmosDB
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/cosmosdb.svg" alt="cosmosdb logo" width="100"></img>

### EntityFrameworkCore
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/netcore.png" alt="postgresql logo" width="100"></img>

### InMemory
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/netcore.png" alt="postgresql logo" width="100"></img>

### Postgres
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/postgresql.jpg" alt="postgresql logo" width="100"></img>

### Sqlite
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/sqlite.gif" alt="sqlite logo" width="100"></img>

### SQL Server
<img src="https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/img/sqlserver.png" alt="sqlserver logo" width="100"></img>

## Other Supported Database Engines
EntityFrameworkCore works with several more different database engines, however we have not implemented them all. 
We have identified those above as the most commonly used providers. If you think we should add more, let us know!

# Implementing a New Database Provider
We suggest using one of our existing storage providers as a guide, such as ServiceBricks.Storage.EntityFrameworkCore or ServiceBricks.Storage.MongoDb.
You should only need a single files to implement the **IStorageRepository** interface and repository pattern, along with a couple other supporting files (such as a Module class).

## Things to Keep in Mind
You will need to implement a solution to keep your database and data model in sync.
EntityFrameworkCore utilizes migrations for their supported databases.
If you do not use an ORM or migrations tool, you will have the manually keep track of and version the database changes yourself as you release updates.

# Copyright Statement
* Names, logos and images used above are owned by their respective copyright owners and not by us. They do not endorse us or our products. We simply use their products in our platform.


