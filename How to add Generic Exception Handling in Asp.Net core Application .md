# How to add Generic Exception Handling in Asp.Net core Application.

# Problem
Exception handling at each controller or service, makes the code a bit messy and untidy. To cope up with the issue we can create our own generic exception handling middleware.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
By creating our own middleware and calling it in the pipeline in our configuration method. 

# Solution

First we have to create a model class for our return message, in which we will be returning the exception along with the message code to our end user.

For this example we will name our model class "ApiException".
```
 public class ApiException
    {
        public ApiException(int statusCode, string message = null)
        {
            StatusCode = statusCode;
            Message = message;
        }

        public int StatusCode { get; set; }
        public string Message { get; set; }
    }
```
Our Model will look something like this having a constructor.

After that we will create an Exception Middleware class that will catch the exception and fill up the model so we can return it to the user.
```
    public async Task InvokeAsync(HttpContext context)
        {
            try
            {
                await _next(context);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, ex.Message);
                context.Response.ContentType = "application/json";
                context.Response.StatusCode = (int)HttpStatusCode.InternalServerError;

                var response = new ApiException(context.Response.StatusCode, ex.Message);

                var options = new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

                var json = JsonSerializer.Serialize(response, options);

                await context.Response.WriteAsync(json);
            }
        }
```
Here InvokeAsync is the function/method inside our Middleware class.
        
next is the delegate, specifying what will come after the execution of the middleware.
        
The status code will always be 500 in this case.
        
Now let's move to the Statup class.
        
In the top of the configuration method in the startup class we have to add the following code:
```    
        app.UseMiddleware<ExceptionMiddleware>();
```
Now that everything is set, we are good to go.
