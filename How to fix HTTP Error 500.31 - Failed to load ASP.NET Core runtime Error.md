# Problem
How to fix HTTP Error 500.31 - Failed to load ASP.NET Core runtime Error while configuration of a new ASP.net core site in IIS ?

# Environment
Visual Studio 2022, Asp.Net Core 6.0, C#, MVC

# How you fix it
While configuration of a new .net core website in IIS, the appropriate runtime must be installed on that machine

# Solution
For ASP.net 6.0, you need to install dot net hosting 6.0.1.2

## After installation, recycle the Applicaiton pool and run the application again
