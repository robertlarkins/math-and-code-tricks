# Migrations

## Change column value
If a column value needs to be changed, such as a nullable column should have empty string values, then the following can be done:

1. Run `Add-Migration`
2. Add the SQL code as a const to the Migration, and as a SQL call:
   ```C#
   public partial class Set_MyTable_MyColumn_Nulls_To_Empty_String : Migration
   {
       private const string SetMyColumnAsEmptyStringWhereNull = @"UPDATE my_table SET my_column = '' WHERE my_column is null;";
       private const string SetMyColumnAsNullWhereEmptyString = @"UPDATE my_table SET my_column = null WHERE my_column = '';";

       protected override void Up(MigrationBuilder migrationBuilder)
       {
           migrationBuilder.Sql(SetMyColumnAsEmptyStringWhereNull);
       }

       protected override void Down(MigrationBuilder migrationBuilder)
       {
           migrationBuilder.Sql(SetMyColumnAsNullWhereEmptyString);
       }
   }
   ```

> Note:
> In the nullable case, ideally the column would be changed to `nullable = false` first.


## Change column type
If a column type needs to be changed there are a couple of options.
The first step is to make modifications to the CodeFirst entities, and run an `Add-Migration`.

If EF Core can't automatically convert the column type, then the better approach is to modify the Migration file to add in custom code for performing the migration.

For example, if the created migration code looks like this:
```C#
public partial class Change_Column_UniqueCode_To_Guid : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        // Either empty, or automatically generated migration code
        // to update the database state from the last migration to this one.
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        // Either empty, or automatically generated migration code
        // to revert the database state from this migration back to the last one.
    }
}
```
and the desire is to convert a string column with guid values in it to a guid column (uuid in postgres), then modify the migration code to look something like this (for a postgres database):
```C#
public partial class Change_Column_UniqueCode_To_Guid : Migration
{
    private const string UniqueCodeStringToGuid = @"ALTER TABLE ""user"" ALTER COLUMN unique_code TYPE uuid USING unique_code::uuid;";
    private const string UniqueCodeGuidToString = @"ALTER TABLE ""user"" ALTER COLUMN unique_code TYPE text USING unique_code::text;";

    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.Sql(UniqueCodeStringToGuid);
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.Sql(UniqueCodeGuidToString);
    }
}
```
In the sql code remember to double quote table and column names that need it, such as if they are uppercase, or are keywords.

Then run `Update-Database` to apply the migration.

Alternatively, you can go to the database and apply the conversion directly, but this is not desirable as
the migration may not be able to automatically run if used in the future due to this manual step.

> Note:
> This approach will only work for the particular type of database (postgres in this case).
> The link to the msdn doc below describes how to accommodate multiple database 

More Info:
 - https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/operations


## Custom Mapping of Column to Another Column

If a table `purchase` had a column `purchase_date_time` of type `timestamptz` (which for NodaTime could be OffsetDateTime), and another column `financial_year` of type int was to be added, which is extracted from `purchase_date_time`, then the following steps could be done for EF CodeFirst:


1. Add the new property to the entity, this might be a straight FinancialYear column of type `int`, or it might be wrapped in a ValueType.
   ```C#
   public FinancialYear FinancialYear { get; private set; } = null!;
   ```

2. Add the property to `PurchaseConfiguration`:
   ```C#
   builder.Property(p => p.FinancialYear)
       .HasConversion(p => p.Value, p => FinancialYear.Create(p).Value);
   ```

3. Run `Add-Migration` which will create a migration file similar to this:
   ```C#
    public partial class Add_FinancialYear_Column_To_Purchase : Migration
    {
        protected override void Up(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.AddColumn<int>(
                name: "financial_year",
                table: "purchase",
                nullable: false,
                defaultValue: 0);
        }

        protected override void Down(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.DropColumn(
                name: "financial_year",
                table: "purchase");
        }
    }
   ```
   
4. The next step is to create some SQL to extract the financial year value from the `purchase_date_time` column into the new `financial_year` column, this could look something like this:
   ```C#
    public partial class Add_FinancialYear_Column_To_Purchase : Migration
    {
        private const string SetFinancialYear = @"UPDATE purchase SET financial_year = EXTRACT(year FROM (purchase_date_time + INTERVAL '9 months'));";
    
        protected override void Up(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.AddColumn<int>(
                name: "financial_year",
                table: "purchase",
                nullable: false,
                defaultValue: 0);
                                
            migrationBuilder.Sql(SetFinancialYear);
        }

        protected override void Down(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.DropColumn(
                name: "financial_year",
                table: "purchase");
        }
    }
   ```

5. Run `Update-Database` to apply these changes.
