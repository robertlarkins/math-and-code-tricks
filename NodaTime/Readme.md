# NodaTime

## NpgSql setup

To allow the NodaTime Date/Time objects to be directly used with PostGres the following steps are needed:

Install the `Npgsql.EntityFrameworkCore.PostgreSQL.NodaTime` NuGet package to the project where the startup.cs file lives.

Go to `startup.cs` or where the location(s) where the line `builder.UseNpgsql("my_connection_string);` resides and change it to
`builder.UseNpgsql("my_connection_string", o => o.UseNodaTime());`.

See: https://www.npgsql.org/efcore/mapping/nodatime.html

