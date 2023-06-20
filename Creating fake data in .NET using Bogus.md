# Problem
Creating fake data in .NET using Bogus


# Environment
Visual Studio 2022

# Solution
## Follow the following steps to creating fake data for testing process


## Step 1
Add the Bogus NuGet package to your console application

```
dotnet add package Bogus
```

## Step 2
Define rules

```
var fakeDotNetDemo = new Faker<Student>()
	.RuleFor(s => s.Id, f=> f.IndexFaker)
	.RuleFor(s => s.FullName, f=> f.Name.FullName)
	.RuleFor(s => s.Country, f=> f.Address.Country)
```

## Step 3
Call generate method

```
var fakeDataDemo = fakeDotNetDemo.Generate();

var json = JsonSerializer.Serializer(fakeDataDemo);
Console.WriteLine(json)

public class Student
{
	public int Id {get; set;}
	public string? FullName {get; set;}
	public string? Country {get; set;}
}
```

## Step 4
Generate N'Fake Entries

```
var fakeDataDemo = fakeDotNetDemo.Generate(2);
var json = JsonSerializer.Serializer(fakeDataDemo);
Console.WriteLine(json)

//Output will be like this
[{"Id":0. "FullName": "Jon Beier", "Country": "Japan"},
{"Id":1. "FullName": "Matt Schmit", "Country": "USA"},
{"Id":2. "FullName": "Victor Frankl", "Country": "Austria"}]
```

