# Add Swagger in Asp.Net Core Application

# Problem
If you are using any asp.net version before 6 then chances are that swagger is not available by default.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
By installing the Swashbuckle package & by adding some code to our startup.

# Solution
### Install required package
```
dotnet add package Swashbuckle.AspNetCore
```
### Add the following code in ConfigureServices Method in our Startup class

```
services.AddSwaggerGen();
services.AddSwaggerGen(c =>
{
	c.SwaggerDoc("v1", new OpenApiInfo{
		Version = "v1",
		Title = "Implement Swagger UI",
		Description = "A simple example to Implement Swagger UI",
	});
});
```

### Then in Configure Method of the Startup.cs file add the following code:
```
app.UseSwagger();
app.UseSwaggerUI(c =>{
	c.SwaggerEndpoint("/swagger/v1/swagger.json", "Showing API V1");
});
```

##### Now visit the url localhost:{port}/swagger/index.html and you are good to go.
