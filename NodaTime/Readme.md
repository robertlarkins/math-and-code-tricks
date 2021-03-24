# NodaTime

## NpgSql setup
To allow the NodaTime Date/Time objects to be directly used with PostGres the following steps are needed:

Install the `Npgsql.EntityFrameworkCore.PostgreSQL.NodaTime` NuGet package to the project where the startup.cs file lives.

Go to `startup.cs` or where the location(s) where the line `builder.UseNpgsql("my_connection_string);` resides and change it to
`builder.UseNpgsql("my_connection_string", o => o.UseNodaTime());`.

See: https://www.npgsql.org/efcore/mapping/nodatime.html

## Register IClock with AutoFac
To allow the IClock to have a concrete object injected for it, AutoFac needs a type registered against it.
As the NodaTime `SystemClock` object does not have a constructor to instantiate it, `SystemClock.Instance` needs to be called.
Therefore, the AutoFac code for doing the registration is
```C#
builder.RegisterInstance(SystemClock.Instance).As<IClock>().SingleInstance();
```
