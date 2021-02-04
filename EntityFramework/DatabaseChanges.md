# Database Changes

## Renaming Tables
Simply renaming an entity class and running `add-migration` will cause the existing table to be dropped and a new table to be added,
without recoupling the table to any other tables it is connected to.

To make the change requires two steps (this is for when FluentAPI is used):

1. Temporarily add `modelBuilder.Entity<YourEntity>().ToTable("new_name");` to the entity's configuration, then run `add-migration`.
   This creates a migration that renames the table and the associated foreign keys, allowing existing data to be maintained.
   Once done the `ToTable()` addition can be removed.
2. Rename the entity class to the desired name.
   
See:
 - https://stack247.wordpress.com/2015/06/18/rename-table-and-column-name-in-ef-code-first/
 - https://www.learnentityframeworkcore.com/configuration/fluent-api/totable-method
 - https://stackoverflow.com/questions/21656617/entity-framework-code-first-changing-a-table-name
 
