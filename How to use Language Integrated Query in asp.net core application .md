# How to use Language Integrated Query in asp.net core application

# Problem
Sometimes in our application we have to returned filtered data through our api. 
Getting all the data from database and then filtering in code can be a time consuming operation. 
To get ahead of this issue we can use LINQ.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
 LNQ is a feature in C# that allows us to perform queries on different data sources. We can utilize it to our advantag in this case too.

# Solution
First we have to add a refernce to the System.Linq namespace 
```
using System.Linq;
```

We can use it in our repository or directly in our API endpoint, whereever we are calling our DBContext class.
For this example let me show you how we can use it in our repository class in an inline function.
```
public async Task<List<Customers>> GetMaleCustomers() => await _dbContext.Customers.Where(x => x.CustomerGender == "Male");
```
This will only return male customers from our database. In doing so will save us a lot of time and processing.
