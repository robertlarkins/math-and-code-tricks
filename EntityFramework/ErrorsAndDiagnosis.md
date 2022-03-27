# Errors And Diagnosis

Provides a list of different errors that have been encountered if EF and their solutions.


## `Unable to create an object of type 'MyContext'.`

Add a parameterless constructor to the *MyContext.cs* file.

See
- https://stackoverflow.com/questions/57745481/unable-to-create-an-object-of-type-mycontext-for-the-different-patterns-suppo


## `No database provider has been configured for this DbContext.`

See:
- https://www.koskila.net/fixing-no-database-provider-has-been-configured-for-this-dbcontext-in-entity-framework-core/


## `No suitable constructor was found for entity type 'ForeignKey'.`

Domain entities used as DB models need a protected parameterless constructor.
