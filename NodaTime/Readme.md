# NodaTime

## NpgSql setup
To allow the NodaTime Date/Time objects to be directly used with PostGres the following steps are needed:

Install the `Npgsql.EntityFrameworkCore.PostgreSQL.NodaTime` NuGet package to the project where the startup.cs file lives.

Go to `startup.cs` or where the location(s) where the line `builder.UseNpgsql("my_connection_string);` resides and change it to
`builder.UseNpgsql("my_connection_string", o => o.UseNodaTime());`.

See: https://www.npgsql.org/efcore/mapping/nodatime.html

### Converting existing database columns

#### Date/Time Column Conversion
If using EF Core with NpgSql and FluentAPI and you want to convert an existing column from timestamp to timestamptz (which seems to be [recommended][1]), then the following needs to be done:

```C#
private const string TimeStampToTimeStampTZ = @"ALTER TABLE my_table ALTER COLUMN my_timestamp_column TYPE timestamptz USING my_timestamp_column AT TIME ZONE 'Pacific/Auckland'";
private const string TimeStampTZToTimeStamp = @"ALTER TABLE my_table ALTER COLUMN my_timestamp_column TYPE timestamp(0) USING my_timestamp_column::timestamp";

protected override void Up(MigrationBuilder migrationBuilder)
{
  // Manuually added code
  migrationBuilder.Sql(TimeStampToTimeStampTZ);
  
  // Automatically generated code will be something like:
  migrationBuilder.AlterColumn<OffsetDateTime>(
                name: "my_timestamp_column",
                table: "my_table",
                nullable: false,
                oldClrType: typeof(DateTime),
                oldType: "timestamp without time zone");
}

protected override void Down(MigrationBuilder migrationBuilder)
{
  // Manuually added code
  migrationBuilder.Sql(TimeStampTZToTimeStamp);
  
  // Automatically generated code will be something like:
  migrationBuilder.AlterColumn<DateTime>(
                name: "inspection_start_date_time",
                table: "inspection",
                type: "timestamp without time zone",
                nullable: false,
                oldClrType: typeof(OffsetDateTime));
}
```

For converting a timestamp to date the following can be done:

```C#
private const string TimeStampToDate = @"ALTER TABLE my_table ALTER COLUMN my_date_column TYPE date USING my_date_column::date";
private const string DateToTimeStamp = @"ALTER TABLE my_table ALTER COLUMN my_date_column TYPE timestamp(0) USING my_date_column::timestamp";
```

See:
 - https://dba.stackexchange.com/questions/134385/convert-postgres-timestamp-to-timestamptz

See [here](EntityFramework/Migrations.md#change-column-type) for more details on the migrations for changing a column type.


## Register IClock with AutoFac
To allow the IClock to have a concrete object injected for it, AutoFac needs a type registered against it.
As the NodaTime `SystemClock` object does not have a constructor to instantiate it, `SystemClock.Instance` needs to be called.
Therefore, the AutoFac code for doing the registration is
```C#
builder.RegisterInstance(SystemClock.Instance).As<IClock>().SingleInstance();
```

## WebAPI NodaTime serialisation
To allow the NodaTime objects to be serialised into recognised format, such as an ISO 8601 string, there are two possible NuGet packages needed,
either `NodaTime.Serialization.JsonNet` which is used for Json.NET (Newtonsoft.Json) or `NodaTime.Serialization.SystemTextJson` for Microsoft's System.Text.Json.
The needed NuGet package is then installed into the project that contains the Asp.Net controllers.

Then in `Startup.cs` add the following:

For `NodaTime.Serialization.SystemTextJson` use the following:
```C#
services.AddControllers()
  .AddJsonOptions(options =>
  {
    options.JsonSerializerOptions.ConfigureForNodaTime(DateTimeZoneProviders.Tzdb);
  });
```

See: 
- https://nodatime.org/3.0.x/userguide/serialization
- https://www.debugcn.com/en/article/41235923.html
- https://stackoverflow.com/questions/64681525/how-to-setup-nodatime-json-serializer-in-asp-net-core-3-1

## SwashBuckle NodaTime Type Representation
Swagger does not display the NodaTime type correctly in its example.
For example, the LocalDate by default is given as
```
"someLocalDate": {
    "calendar": {
        "id": "string",
        "name": "string",
        "minYear": 0,
        "maxYear": 0,
        "eras": [
            {
              "name": "string"
            }
          ]
        },
    "year": 0,
    "month": 0,
    "day": 0,
    "dayOfWeek": 0,
    "yearOfEra": 0,
    "era": {
        "name": "string"
    },
    "dayOfYear": 0
}
```
when it should simply be
```
"someLocalDate": "2020-07-21"
```

To fix this there are (at least) two possible NuGet packages that can be installed:
 - [`Swashbuckle.NodaTime.AspNetCore`](https://github.com/buvinghausen/Swashbuckle.NodaTime.AspNetCore)
 - [`MicroElements.Swashbuckle.NodaTime`](https://github.com/micro-elements/MicroElements.Swashbuckle.NodaTime)
   This is the better choice if SystemTextJson is being used.

These links provide instructions for how to setup.



[1]: https://wiki.postgresql.org/wiki/Don't_Do_This#Date.2FTime_storage)
