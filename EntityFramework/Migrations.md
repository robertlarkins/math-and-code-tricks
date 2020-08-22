# Migrations

## Change column type
If a column type needs to be changed, EF Core may not be able to do this itself.
To achieve this, make the modifications to the CodeFirst entities, and run an `Add-Migration`.

Before running `Update-Database`, go to the database table and convert the column directly.
A PostgreSQL example can be seen in the PostgresSQL documentation.
