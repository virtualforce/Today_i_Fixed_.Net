# How to implement Serilog in Asp.Net Core application for saving logs to files

# Problem
Logging is the process of saving application events. By saving Logs in a file we can keep track of the activity on the systems and networks of your organization, notice unusual activity, scan vulnerabilities and enhance the security posture of your organization.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
By installing and using the "Serilog.Extensions.Logging.File" package from our nuget package manager gallery.

# Solution
In any Asp.Net Core application, logging is provided to us by default. If it is not provided in any older version, we can download the following packages to configure it ourselves:
* Microsoft.Extensions.Logging
* Microsoft.Extensions.Logging.Console
* Microsoft.Extensions.Logging

Now in the startup we have to add the follwoing code:
```
    services.AddSingleton<ILoggerFactory, LoggerFactory>();
    services.AddLogging();
```

Now logging configuration is already setup in appsettings.json. If it is not, add the following code in appsettings.json:
```
    "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
    }
```

Now to save all the logs in a file we have to install the "Serilog.Extensions.Logging.File" package. 

Afterwards we have to add the following code in our startup's configure method.
```
    loggerFactory.AddFile(Configuration.GetSection("Logging").GetValue<string>("../myselectedpath"));
```
Now we can add logging anywhere in our project and it will be automatically saved to our file.
