# How to connect database to a asp.net web project using Entity Framework Core

# Problem
Even small level practice project's today require the usage of database. Entity Framework has made the connection to database a lot simpler and easy. You can connect MySQL, MSSQL, MariaDB, PostgreSQL, etc. using it.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
By following the steps given in the solution below:

# Solution 
Download the mentioned Entity Framework Core nuget packages using the nuget package manager:
* Microsoft.EntityFrameworkCore
* Microsoft.EntityFrameworkCore.SqlServer
* Microsoft.EntityFrameworkCore.Tools

Create a DBContext class. This is used to interact with the database. The code in the class will look like this:
```
public class MyDbContext : DbContext
{
    public MyDbContext(DbContextOptions<MyDbContext> options) : base(options)
    {
    }
    public DbSet<Customer> Customers { get; set; }
    public DbSet<Order> Orders { get; set; }
}
```
Here the customer and Order are the names of your entities or Tables inside the database.

In next step you have to configure the connection string for your database. You can do it in your startup class using the following code:
```
services.AddDbContext<MyDbContext>(options =>

    options.UseSqlServer(Configuration.GetConnectionString("MyConnectionString")));
```
Your database is successfully setup. Now you can inject the DBContext in your controller class using a simple constructor injection.
