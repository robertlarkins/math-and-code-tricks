# Querying Database

## EF.Functions
EF.Functions is a namespace in Entity Framework that provides methods that can be translated to database functions.

For example if you wanted to find names that contain `Jo` in the firstname with any casing:
```C#
context.Names.Where(x => x.FirstName.ToLower().Contains("jo")).ToList();
```
can be converted to:
```C#
context.Names.Where(x => EF.Functions.ILike(x.FirstName, "%Jo%")).ToList();
```
which is much faster as it better leverages the built in SQL functions.

See:
 - https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.ef.functions?view=efcore-5.0

## With ValueObjects



## Client-Side Evaluation


 - https://docs.microsoft.com/en-us/ef/core/querying/client-eval
 - https://stackoverflow.com/questions/61691178/no-backing-field-could-be-found-for-property-of-entity-type-and-the-property-doe

## Error Messages

### Error 1

> No backing field could be found for property 'Abc' of entity type 'Xyz' and the property does not have a getter.
Entity framework is unable to convert the comparison of an owned-entity into SQL. The solution is to explicitely compare each property.

See: https://stackoverflow.com/questions/61691178/no-backing-field-could-be-found-for-property-of-entity-type-and-the-property-doe

### Error 2

> The LINQ expression
> *The Entity Framework LINQ expression*
> could not be translated. Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().

This occurs because Entity Framework is unable to transform the LINQ query into SQL. With using ValueObjects this can occur because it is unable to convert the equality comparison into SQL.
One fix for this is to modify the query to compare the underlying values, so if the original query looked like this:

```C#
var someAddress = Address.Create(123, "Fake St", SpringField).Value;

var foundCompany = await context.Companies.SingleOrDefaultAsync(company => company.Address == someAddress);
```
then it could be changed to

```C#
var someAddress = Address.Create(123, "Fake St", SpringField).Value;

var foundCompany = context.Companies.SingleOrDefault(company =>
    company.Address.StreetNumber == someAddress.StreetNumber &&
    company.Address.StreetName == someAddress.StreetName &&
    company.Address.City == someAddress.City);
```

See below for what `Company` and `Address` look like.

Another approach is to perform explicit client-side evaluation, which pulls the data from the db into client-side memory and evaluates it there.
This is not recommended as it can have a detrimental impact on performance if there is a lot of data. This requires adding something like `ToList()`
into the query to pull back the data. It could be done like this:

```C#
var someAddress = Address.Create(123, "Fake St", SpringField).Value;

var foundCompany = context.Companies.ToList().SingleOrDefault(company => company.Address == someAddress);
```

Alternatively the query could be broken into two or more stages, with the first stage bringing back data restricted to a piece of the query.
This may allow a client-side evaluation to be performed with a reduced dataset.

This may be necessary if the query is being peformed on a Navigation Property of an entity, in which case, find the Navigation Property entity.
This retrieved entity (or entity list) can then be used in a subsequent query call, and the resulting retrieved data being evaluated on the client-side.

For example if we were wanting to find a company based on say CompanyOwner and Address, and CompanyOwner was a Navigation Property on Company.
We could first query for CompanyOwner, and use the found entity to restrict the query on Company. While this might be two calls, it may avoid
needing to do client-side evaluation.

> Note:
> Some ValueObjects, such as ValueObjects for wrapping a DateTime value, do not appear to work in an Entity Framework query, even when using their underlying value.
> In this situation client-side seems to be the only solution.


See:
 - https://stackoverflow.com/questions/58074844/ef-linq-error-after-change-from-dotnet-core-2-2-6-to-3-0-0
 - https://docs.microsoft.com/en-us/ef/core/querying/client-eval
 - https://www.thecodebuzz.com/query-linq-expression-could-not-be-translated/


### Company Entity and Address ValueObject

```C#
public class Company : Entity
{
    public static reasonly Company Acme = new Company(1, "Acme", Address.Create(123, "Fake St", "SpringField").Value);
    public static reasonly Company Xyz = new Company(1, "Xyz", Address.Create(99, "Alphabet Road", "Letterton").Value);

    protected Company()
    {
    }
    
    private Company(
        int id,
        string name,
        Address address)
        : base(id)
    {
        Name = name;
        Address = address;
    }
    
    public string Name { get; private set; } = string.Empty;
    
    public virtual Address Address { get; private set; } = null!;
}

public class Address : ValueObject
{
    protected Address()
    {
    }
    
    private Address(
        int streetNumber,
        string streetName,
        string city)
    {
        StreetNumber = streetNumber;
        StreetName = streetName;
        City = city;
    }
    
    public int StreetNumber { get; }
    
    public string StreetName { get; } = string.Empty;
    
    public string City { get; } = string.Empty;
    
    public static Result<Address> Create(
        int streetNumber,
        string streetName,
        string city)
    {
        return new Address(streetNumber, streetName, city);
    }
    
    protected override IEnumerable<object> GetEqualityComponents()
    {
        yield return StreetNumber;
        yield return StreetName;
        yield return City;
    }
}
```

# Value Conversion (FluentAPI Configuration)
- https://docs.microsoft.com/en-us/ef/core/modeling/value-comparers
- https://docs.microsoft.com/en-us/ef/core/modeling/value-conversions
