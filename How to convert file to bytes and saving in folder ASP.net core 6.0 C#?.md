# Problem
How to convert a file to bytes from choose file options and save it in folder in ASP.net core 6.0 C#

# Environment
Visual Studio 2022, Asp.Net Core 6.0, C#, MVC

# How you fix it
With the help of Async and Await, you can convert the files to byte in asp.net core c#.

# Solution
First you need to get all the files from razor view using form submission and choose file options


```
[HttpPost]        
public async Task<IActionResult> AddNewRecord( List<IFormFile> files)        
{
	long size = files.Sum(f => f.Length); 
            
            foreach (var formFile in files)
            {
                int fileTypeID = 0; 
                if (formFile.Length > 0)
                {
                    byte[] bytes = null;

                    using (var memoryStream = new MemoryStream())
                    {
                        await formFile.CopyToAsync(memoryStream);
                        bytes = memoryStream.ToArray();
                    }
                    
                }
            }
}

```

You can further use this bytes variable to pass to any wcf service to write it in folder
