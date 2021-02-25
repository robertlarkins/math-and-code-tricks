# Querying Database

## With ValueObjects



## Client-Side Evaluation


 - https://docs.microsoft.com/en-us/ef/core/querying/client-eval
 - https://stackoverflow.com/questions/61691178/no-backing-field-could-be-found-for-property-of-entity-type-and-the-property-doe

## Error Messages
> No backing field could be found for property 'Abc' of entity type 'Xyz' and the property does not have a getter.
Entity framework is unable to convert the comparison of an owned-entity into SQL. The solution is to explicitely compare each property.

See: https://stackoverflow.com/questions/61691178/no-backing-field-could-be-found-for-property-of-entity-type-and-the-property-doe

# Value Conversion (FluentAPI Configuration)
- https://docs.microsoft.com/en-us/ef/core/modeling/value-comparers
- https://docs.microsoft.com/en-us/ef/core/modeling/value-conversions
