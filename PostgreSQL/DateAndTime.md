# Understanding Date/Time in PostGres

## `timestamp`

The `timestamp` type does not store any timezone information, so when retrieving it, it simply returns as is.
This does not ensure that all time values are in a consistent timezone, such as UTC.
In fact, the PostGres recommendations say _NOT_ to use TimeStamp as a type, rather `timestamptz` should be used.
More about this can be read [here](https://wiki.postgresql.org/wiki/Don't_Do_This#Date.2FTime_storage), but essentially,
`timestamp` could be for or from any timezone, date/time comparisons or conversions could give the wrong answer.
Where as `timestamptz` is a single moment in time.

## `timestamptz`

The `timestamptz` type in PostGres stores a single moment (or instant) in time as UTC. It is also known as _TIMESTAMP WITH TIME ZONE_.
This allows comparisons and conversions to be performed that take into consideration with respect to a timezone.

When using tools such as DBeaver, it is best to understand how `timestamptz` is displayed by default.
For example DBeaver displays `timestamptz` in the local system time, if the preference is for DBeaver to display this as
UTC (the timezone used for internally storing `timestamptz`), or some other timezone, this can be changed in DBeaver.ini by adding or changing
```
-Duser.timezone=UTC
```
See:
- https://stackoverflow.com/a/52575643/1926027

An important thing to understand about how PostGres operates with `timestamptz` is that it does not store a timezone with the value in the database.
Rather the `timestamptz` gets converted on the fly to the TimeZone specified in the PostGres database settings.
It is this timezone that provides the offset that is given on the C# types such as NodaTime's OffsetDateTime.
As it is a single instance in time in UTC it can be converted to other timezones easily.

See:
- https://www.postgresql.org/docs/9.1/datatype-datetime.html
- https://www.npgsql.org/doc/types/nodatime.html#mapping-table
- https://www.postgresqltutorial.com/postgresql-timestamp/
- https://www.enterprisedb.com/postgres-tutorials/postgres-time-zone-explained
