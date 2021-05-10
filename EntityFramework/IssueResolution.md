# Issue Resolution
This document documents issues that may occur when using Entity Framework, why they occur and how they can be fixed.

## `The instance of entity type cannot be tracked`

When working with Entity types there will be times their values need to be updated.
For example, if there are two Entities, Appointment and AppointmentStatus, and Appointment has an AppointmentStatus property, then AppointmentStatus would be changeable.
AppointmentStatus would likely have statically defined values that are seeded into the database.
Entities with static values generally occurs for status or type entities (https://fanzootechnology.com/blog/static-entity-implementation-with-domain-driven-design/).

```C#
public class Appointment : Entity
{
    public AppointmentStatus AppointmentStatus { get; private set; } = null!;
    
    public void SetAppointmentStatus(AppointmentStatus newStatus) => AppointmentStatus = newStatus;
}

public class AppointmentStatus : Entity
{
    public static readonly Planned = new(1, "Planned");
    public static readonly Occurring = new(2, "Occurring");
    public static readonly Finished = new(3, "Finished");
    
    // The rest of the needed code
    ...
}
```

However, Entity Framework can throw the following exception:
```
"The instance of entity type 'YourEntity' cannot be tracked because another instance with the same key value for {'Id'} is already being tracked.
When attaching existing entities, ensure that only one entity instance with a given key value is attached.
Consider using 'DbContextOptionsBuilder.EnableSensitiveDataLogging' to see the conflicting key values."
```

This appears to occur (intermittently) when the following situations occur:
 - an entity (lets call it the parent entity) has a static entity as a property
 - the parent entity comes from the database
 - the static entity value is updated with the static entity from code

What happens is EFCore pulls the parent entity from the database, this entity has an object reference to the static entity from the database.
Assigning the same static entity (the one defined in code) to the parent's property causes EFCore to _hold_ both
(the static entity from the db and from code) in the dbcontext, but when saving the context back to the database it is
unable to establish these entities are the same and causes EFCore to throw the exception above.

At present I'm unsure of the exact underlying mechanism for why this issue occurs, or why EFCore can't distinguish these instances as being the same.
This will be something to investigate further.

### The Fix
The simplest fix is when assigning a new static entity value to the parent property (via a method) is to check if the current value is the same as the new value.
If they are the same then do not assign the value.

Overriding the `SaveChangesAsync` method to something like this:

```C#
public override Task<int> SaveChangesAsync(CancellationToken cancellationToken = default)
{
    IEnumerable<EntityEntry> enumerationEntries = ChangeTracker.Entries()
        .Where(x => EnumerationTypes.Contains(x.Entity.GetType()));

    foreach (var enumerationEntry in enumerationEntries)
    {
        enumerationEntry.State = EntityState.Unchanged;
    }

    return base.SaveChangesAsync(cancellationToken);
}
```
unfortunately does not solve the problem as the exception gets thrown on the `enumerationEntries` assignment line.
