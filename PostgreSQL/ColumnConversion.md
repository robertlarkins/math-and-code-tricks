# Column Conversion
The following shows the Postgres syntax for converting the type of a column, along with the existing data, to a new type.

> Note:
> If using a program such as DBeaver, then ensure the table columns section is refreshed to see the update, otherwise it might still falsely indicate it as being text.
> DBeaver can be used to generate the syntax and the changes by going to the table column > View Column, change the Data type and save. This will show the syntax to be run, which can then be persisted.

## Text to UUID
The following converts a text column to uuid.
```
ALTER TABLE table_name
ALTER COLUMN column_name TYPE uuid
USING subject_id::uuid;
```
