# Problem
How to bypass JWT Bearer (Http Authorization header) in Integration Testing?

# Environment
Visual Studio, Asp.Net Core 3.1, C#

# How you fix it
Installing Moq, NUnit, Microsoft.NET.Test.Sdk, NUnit3TestAdapter, MSTest.TestFramework packages.

# Solution
## Installing packages
Install the required packages from the Nuget Package Manager.

## Add the following code in the TestMethod of YourTestClass.cs file

```
var httpContext = new DefaultHttpContext();
httpContext.Request.Headers[HeaderNames.Authorization] = "Bearer token";
```
## Now add the following code in TestMethod file
Here _Service1.Object is the mock object of service required in the constructor of your YourOriginalController.

```
var yourTestController = new YourOriginalController( _Service1.Object, Service2.Object)
            {
                ControllerContext = new ControllerContext()
                {
                    HttpContext = httpContext
                }
            };
```
Now if your run the test case, Request.Headers will not throw an exception. Because you added moq "Bearer" in the request header. 

## Run Test Application
Now run the Testcase from the Test Explorer, you will get no more exception while dealing with the Authorization header.
