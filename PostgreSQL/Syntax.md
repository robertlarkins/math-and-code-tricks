# Postgres Syntax Descriptions

## Excluded table

The `EXCLUDED` keyboard provides a reference to the special *excluded* table.
This table is used when inserting data into a table but a conflict occurs, it allows the conflicted data (the data trying to be inserted) to be referenced and used if necessary.

For example:
```
insert into address (number, street_name, city)
values
(123, 'Fake St', 'Hamilton'),
(456, 'Wrong Pl', 'Auckland'),
on conflict (number, street_name) do update set
  city = EXCLUDED.city;
```

See Also:
 - https://www.postgresql.org/docs/current/sql-insert.html
 - https://wiki.postgresql.org/wiki/What's_new_in_PostgreSQL_9.5#INSERT_..._ON_CONFLICT_DO_NOTHING.2FUPDATE_.28.22UPSERT.22.29
 - https://www.reddit.com/r/PostgreSQL/comments/ghzpfx/what_does_excluded_mean/
 - https://www.postgresqltutorial.com/postgresql-upsert/
