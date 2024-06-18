Пример валидатора:
```csharp
using FluentValidation;
using Ofg.WindowFunctions.DatabaseAccess.Dtos.Shifts;
using Ofg.WindowFunctions.DatabaseAccess.Dtos.ShiftsSchedules;

public class CreateShiftsScheduleDtoValidator : AbstractValidator<CreateShiftsScheduleDto>
{
    public CreateShiftsScheduleDtoValidator()
    {
        RuleFor(x => x.TimeZone)
            .InclusiveBetween(-12, 14)
            .WithMessage("TimeZone must be between -12 and 14.");

        RuleFor(x => x.Shifts)
            .NotNull()
            .WithMessage("Shifts cannot be null.")
            .Must(shifts => shifts.Count > 0)
            .WithMessage("Shifts list must contain at least one shift.");

        RuleForEach(x => x.Shifts).SetValidator(new GetShiftDtoValidator());
    }
}

public class GetShiftDtoValidator : AbstractValidator<GetShiftDto>
{
    public GetShiftDtoValidator()
    {
        RuleFor(x => x.Start)
            .NotEmpty()
            .WithMessage("Shift start time cannot be empty.");

        RuleFor(x => x.End)
            .NotEmpty()
            .WithMessage("Shift end time cannot be empty.");

        RuleFor(x => x.Number)
            .GreaterThan(0)
            .WithMessage("Shift number must be greater than 0.");
    }
}

```

Регистрация в Program.cs:

```csharp
builder.Services.AddFluentValidationAutoValidation()
    .AddFluentValidationClientsideAdapters();

builder.Services.AddValidatorsFromAssemblyContaining<CreateFunctionDtoValidator>();
```
