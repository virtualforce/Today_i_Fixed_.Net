# Problem
We can disable an Azure Function anytime using Azure Portal through its user friendly UI.

What if you are working in localhost and need to disable a particular Azure function?

# How you fix it
Although you don't have any UI to disable a function locally, yet, it is doable by setting disable property in `local.settings.json`

# Solution
- Go to `local.settings.json` file

- Add a new key ` "AzureWebJobs.<MyFunctionName>.Disabled": "1",`

```
{
    "IsEncrypted":  false,
    "Values":  {
                   "WEBSITE_ENABLE_SYNC_UPDATE_SITE":  "true",
                   "FUNCTIONS_EXTENSION_VERSION":  "~4",
                   "AzureAppConfig_LabelFilter":  "DEV",
                   "WEBSITE_OVERRIDE_STICKY_EXTENSION_VERSIONS":  "False",
                   "WEBSITE_CONTENTSHARE":  "forms-20230302104145",
                   "WEBSITE_RUN_FROM_PACKAGE":  "1",
                   "WEBSITE_ADD_SITENAME_BINDINGS_IN_APPHOST_CONFIG":  "1",
                   "FUNCTIONS_WORKER_RUNTIME":  "dotnet",
                   "AzureWebJobs.MyFunctionName.Disabled":  "1",
               }
}
```                
# Environment
Visual Studio 2022, .Net 6, C#, Azure Functions
