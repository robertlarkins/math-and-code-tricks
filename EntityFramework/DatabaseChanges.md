# Database Changes

## Renaming Tables
Simply renaming an entity class and running `add-migration` will cause the existing table to be dropped and a new table to be added,
without recoupling the table to any other tables it is connected to.

To make the change requires two steps (this is for when FluentAPI is used):

1. Temporarily add `modelBuilder.Entity<YourEntity>().ToTable("new_name");` to the entity's configuration, then run `add-migration`.
   This creates a migration that renames the table and the associated foreign keys, allowing existing data to be maintained.
   Once done the `ToTable()` addition can be removed.  
   > Note:  
   > Ensure that the DbSet property in the dbContext.cs reflects the new name.
2. Rename the entity class to the desired name.
   
See:
 - https://stack247.wordpress.com/2015/06/18/rename-table-and-column-name-in-ef-code-first/
 - https://www.learnentityframeworkcore.com/configuration/fluent-api/totable-method
 - https://stackoverflow.com/questions/21656617/entity-framework-code-first-changing-a-table-name
 - https://stackoverflow.com/questions/13296996/entity-framework-migrations-renaming-tables-and-columns

## Renaming Columns
This is similar to renaming tables. Simply changing properties and running `add-migration` will likely cause the existing column to be dropped and readded, causing it to lose whatever data was in it.

To make the change requires two steps (this is for when FluentAPI is used):

1. Temporarily add or change (if already in place) the `HasColumnName("new_column_name")` extension on
`modelBuilder.Entity<YourEntity>().Property(p => p.SomeProperty);` or equivalent FluentAPI.

2. Rename the properties or other items associated with this column.


For example if it is a shadow property in an OwnsOne, when a ValueObject has an Entity:
```C#
builder.OwnsOne(p => p.MyValueObjectProperty, p =>
{
    p.Property<int>("ChildEntityId")
        .HasColumnName("ChildEntityId");
    p.Property(pp => pp.SomeOtherProperty).HasColumnName("another_property");
    p.HasOne(pp => pp.ChildEntity).WithMany()
        .HasForeignKey("ChildEntityId");
});
```
change the value in the `HasColumnName` extension method. Run a migration, then change the shadow property name.

### Navigation Properties Using a Shadow Property for Id

If a column is based on a shadow property, such as if a Navigation Property is defined:
```C#
modelBuilder.Entity<YourEntity>().HasOne(p => p.SomeNavigationProperty).WithMany();
```
then shadow property for the foreign key will be `SomeNavigationPropertyId` (or some autmatically generated equivalent).
To rename this property, add the following FluentAPI:
```C#
builder.Property("SomeNavigationPropertyId").HasColumnName("SomeNavigationPropertyId");
```
if `add-migration` is now run, there should be no migration changes. Now if the name is changed

```C#
builder.Property("SomeNavigationPropertyId").HasColumnName("MyNewId");
```
the migration should rename the column to the new name.

> Note:
> This column renaming might have to take place before other changes, otherwise `add-migration` might drop and and the column, potentially losing data.

See:
 - https://github.com/dotnet/efcore/issues/5893
