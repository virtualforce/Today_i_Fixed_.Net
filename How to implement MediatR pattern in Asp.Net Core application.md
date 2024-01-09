# How to implement mediatr pattern in c# .net core application.

# Problem
The Mediator pattern is a behavioral design pattern that helps to reduce direct access between components by introducing a mediator class.
Components maybe a controller class, in our below example we implement mediatr in api controller class.

# Environment
Visual Studio 2019 and above, Asp.Net Core, C#

# How you fix it

Install MediatR NuGet Package.

# Solution

First we need to install the MediatR NuGet package:
```
dotnet add package MediatR
```

Create Request and Handler Classes:
Define your request and handler classes. Requests represent the messages you want to send, and handlers contain the logic to process those requests.
```
// Request class
public class SampleRequest : IRequest<string>
{
    // Request properties, if any
}

// Handler class
public class SampleRequestHandler : IRequestHandler<SampleRequest, string>
{
    public Task<string> Handle(SampleRequest request, CancellationToken cancellationToken)
    {
        // Handle the request and return a result
        return Task.FromResult("Sample response");
    }
}
```

Now register the MediatR in Dependency Injection:
In your Startup.cs file, register MediatR in the dependency injection container.
```
using MediatR;

public void ConfigureServices(IServiceCollection services)
{
    // Other services

    services.AddMediatR(typeof(Startup));
}
```
Use Mediator in Controller:
Inject the IMediator interface into your controller and use it to send requests and receive responses.
```
[ApiController]
[Route("api/[controller]")]
public class SampleController : ControllerBase
{
    private readonly IMediator _mediator;

    public SampleController(IMediator mediator)
    {
        _mediator = mediator;
    }

    [HttpGet]
    public async Task<IActionResult> Get()
    {
        var result = await _mediator.Send(new SampleRequest());
        return Ok(result);
    }
}
```
In above example, send a GET request to the specified endpoint, it will trigger the SampleRequestHandler to process the request and return a response.

Great!!! You've implemented the Mediator pattern using MediatR in a C# .NET Core application.
