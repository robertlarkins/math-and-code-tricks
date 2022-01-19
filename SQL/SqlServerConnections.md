# SQL Server Connections

One way of finding local db instances on your machine is to go to powershell and run
`sqllocaldb.exe i`
which will list the instances.

However, there are other local db servers that can be connected to in SQL Server Management Studio (SSMS),
each of which use `Authentication: Window Authentication`, with server name:
- `(localdb)/MSSQLLocalDB`  
  This is the automatic instance owned by the current user. It is created when SQL Server Express is installed (I believe).
- `localhost` or simply `.`  
  This is the SqlServer instance.

See:
- https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15
- https://stackoverflow.com/questions/42638519/sql-difference-between-localhost-and-localdb-mssqllocaldb
