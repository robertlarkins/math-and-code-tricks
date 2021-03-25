# Migrations

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
