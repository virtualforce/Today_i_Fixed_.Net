# How to Call Store Procedure Directly from our Database in the .NET Core Application Project.

# Problem
In some complex scenarios, where we need combined data from multiple tables in our database, store procedure makes our application a bit faster and helps us reduce latency while fetching the data.

# Environment
Visual Studio, Asp.Net Core, C#, MySQL, MSSQL, MariaDB

# How to achieve it
First, we have to be sure that our application is properly connected to the database, after that we can call the store procedure as an asynchronous function.

# Solution
First we have to create an Entity with all the attributes that are returned from the store procedure. 

Note: that the name of the properties in our entity should match either the names of the fields or their aliases that are returned from the store procedure.

Let's suppose we have an entity DemoCustomer, the entity does not represent any table but the return type fields of a store procedure:
```
      public class DemoCustomers     {
        public string CustomerName { get; set; }
        public string ProfilePicturePath { get; set; }
        public decimal Age { get; set; }
        public string Gender { get; set; }
    }
```
Now it is mandatory to refrence the entity in our "database context", treating it like a table in our database.
```
    public DbSet<DemoCustomers> DemoCustomers { get; set; }
```
Now we just have to call the store procedure as an async function from our repository class by providing the name and parameters for the procedure.
```
    public async Task<DemoCustomers> GetDemoCustomers(Guid customerId)
        {
            var transactions =  await _dbContext.DemoCustomers.FromSqlRaw("call GetDemoCustomers({0})", customerId).ToListAsync();
            return transactions.FirstOrDefault();
        }
```
