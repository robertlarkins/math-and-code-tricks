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
