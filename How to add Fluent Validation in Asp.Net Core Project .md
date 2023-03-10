# How to add Fluent Validation in Asp.Net Core Project.

# Problem
FluentValidation is a .NET library that provides an easy way to create validation rules for the properties in your models. 
With FluentValidation, we create a separate public class for our rules, encouraging maintainability and preventing code bloat issues.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
Using the .Net fluent Validation Library

# Solution
First we have to install the "FluentValidation.AspNetCore" package from our nuget package manager gallery.

Afterwards for the package to work, we have to add the following code in our startup class:
```
    builder.Services.AddControllers()
        .AddFluentValidation(c =>
        c.RegisterValidatorsFromAssembly(Assembly.GetExecutingAssembly()));
```

Fluent Validation is applied on the models, so we can assume for this demonstration that there is a User model.

In the User model we have User details, including FirstName and City.
Our Model will look something like this:
```
     public class User
    {
        public string FirstName { get; set; } = string.Empty;
        public string City { get; set; } = string.Empty;
    }
```

Now let's create a new class named as "UserValidator".

UserValidator will have the following code inside of it.
```
     public class UserValidator : AbstractValidator<User>
    {
        public UserValidator()
        {
            RuleFor(u => u.FirstName)
                .NotEmpty().WithMessage("{PropertyName} field can not be empty")
                .MinimumLength(10).WithMessage("Length ({TotalLength}) of {PropertyName} is less than 10")
                
            RuleFor(u => u.City)
               .NotEmpty().WithMessage("{PropertyName} field can not be empty").
               MinimumLength(10).WithMessage("Length ({TotalLength}) of {PropertyName} is less than 10");
        }
    }
```
Here the FirstName and City can not be empty and the minimum length of both should be greater than 10 letters.

Also we are returning our own defined messages, incase the validations are not fulfilled.
