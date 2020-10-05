# Fluent Validations

## Checking multiple items exist in database
The following shows different approaches for how a list of items can be be checked to see if they exist in the database within an AbstractValidator class.
These are based on the dicussion found here: https://github.com/FluentValidation/FluentValidation/issues/1506

### `RuleFor.Custom`
```c#
public class UploadAnimalDetailsCommandValidator : AbstractValidator<UploadAnimalDetailsCommand>
{
    public UploadAnimalDetailsCommandValidator(MyDbContext dbContext)
    {
        RuleFor(x => x).Custom((dto, context) =>
        {
            var animalIds = dto.Animals.Select(x => x.AnimalId);
            var existingAnimalIds = dbContext.Animals.Where(x => animalIds.Contains(x.Id)).ToList();
            var nonexistentIds = animalIds.Except(existingAnimalIds).ToList();

            if (nonexistentIds.Any())
            {
                context.AddFailure(new ValidationFailure("Animals", $"Animals with Ids {string.Join(", ", nonexistentIds)} are unknown."));
            }
        });
    }
}
```

### PropertyValidator
```c#
public class UploadAnimalDetailsCommandValidator : AbstractValidator<UploadAnimalDetailsCommand>
{
    public UploadAnimalDetailsCommandValidator(MyDbContext dbContext)
    {
        RuleFor(x => x.Animals).SetValidator(new AnimalsExistValidator(dbContext));
    }

    private class AnimalsExistValidator : PropertyValidator
    {
        private readonly MyDbContext dbContext;

        public AnimalsExistValidator(MyDbContext dbContext)
            : base("{PropertyName} with Ids {AnimalIds} are unknown.")
        {
            this.dbContext = dbContext;
        }

        protected override bool IsValid(PropertyValidatorContext context)
        {
            if (!(context.PropertyValue is IList<UploadAnimalsDetailsCommand> animalList))
            {
                return false;
            }

            var animalIds = animalList.Select(x => x.AnimalId);
            var existingAnimalIds = dbContext.Animals.Where(x => animalIds.Contains(x.Id)).Select(x => x.Id);
            var nonexistentIds = animalIds.Except(existingAnimalIds).ToList();

            if (nonexistentIds.Any())
            {
                context.MessageFormatter.AppendArgument("AnimalIds", string.Join(", ", nonexistentIds));
                return false;
            }

            return true;
        }
    }
}
```

### ValidateAsync
This is an exensibility point in a FluentValidations AbstractValidator class.
```c#
public class UploadAnimalDetailsCommandValidator : AbstractValidator<UploadAnimalDetailsCommand>
{
    private readonly MyDbContext dbContext;

    public UploadAnimalDetailsCommandValidator(MyDbContext dbContext)
    {
        this.dbContext = dbContext;
    }
    
    public override async Task<ValidationResult> ValidateAsync(
      ValidationContext<UploadAnimalDetailsCommand>,
      CancellationToken cancellation = default(CancellationToken))
    {
        var result = await base.ValidateAsync(context, cancellation);
        var animalDetails = context.InstanceToValidate;
        
        var animalIds = animalDetails.Animals.Select(x => x.AnimalId);
        var existingAnimalIds = dbContext.Animals.Where(x => animalIds.Contains(x.Id)).ToList();
        var nonexistentIds = animalIds.Except(existingAnimalIds).ToList();

        if (nonexistentIds.Any())
        {
            result.Errors.Add("Animals", $"Animals with Ids {string.Join(", ", nonexistentIds)} are unknown.");
        }

        return result;
    } 
}
```

## `SetValidator`
This is used to provide a custom validator for a property (typically a class that needs its own validations). If this property can be null, then there should be a `NotNull()` check before `SetValidator`, as `SetValidator` requires a non-null input, and wont be called on an input that is null.

See:
 - https://github.com/FluentValidation/FluentValidation/issues/1006
 - https://docs.fluentvalidation.net/en/latest/custom-validators.html#reusable-property-validators

When using nullable reference types with `SetValidator`, then the type on the abstract validator also needs to be nullabe to match the property. But the `NotNull` check prior to `SetValidator` is still needed. The Validator is still entered if the property is null, but doing `RuleFor(x => x).NotNull()` doesn't capture the null case. The Rules in the validator need to use the _null forgiving operator_ (`!`) to stop compiler warnings, but will work when the property is either null or non-null. Eg: `RuleFor(x => x!.SubProperty).GreaterThanOrEqualTo(0)`.

See:
 - https://github.com/FluentValidation/FluentValidation/issues/1168


# Reason why the validator might not work
The `AbstractValidator` works on the param that is past into the API action, not on the object passed by Mediatr. This is because FluentValidations intercepts the param before the Action is called. Therefore the validator must be defined for the param type. If the param type is an int (such as for an id), then the `AbstractorValidator` generic type should be `int`, but this isn't appropriate as it will be applied to all API endpoints that take in an int param.

For simple validations, such as whether only an id is passed, can be checked in the handler to see if it corresponds to an actual object in the database.

Further Reading:
 - https://github.com/FluentValidation/FluentValidation/issues/184
 - https://github.com/FluentValidation/FluentValidation/issues/230
 
