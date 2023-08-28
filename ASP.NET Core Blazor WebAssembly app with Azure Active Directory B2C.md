# Problem
ASP.NET Core Blazor WebAssembly app with Azure Active Directory B2C

# Environment
Visual Studio, Asp.Net Core 6, Azure

# How you fix it
Go through the following document to add Azure B2C authentication in your blazor webassembly

# Solution
## Create a tenant in Azure
Before proceeding with this document, you must have tenant in azure if not create a tenant in azure first. And then follow these steps.

## Register an AAD B2C app for the Server API app
Navigate to Azure AD B2C in the Azure portal. Select App registrations in the sidebar. Select the New registration button.

![AppRegister](https://github.com/usamabaig202/Today_i_Fixed_.Net/assets/119293426/c8a05348-3c16-4ed0-bd0c-40ea2b222ad8)

Provide a Name for the app (for example, Blazor Server AAD B2C).

![RegisteringAppTemplate](https://github.com/usamabaig202/Today_i_Fixed_.Net/assets/119293426/29de021b-85a4-405f-8f87-3309debd37fa)

For Supported account types, select the multi-tenant option: Accounts in any identity provider or organizational directory (for authenticating users with user flows).

The Server API app doesn't require a Redirect URI in this scenario, so leave the Select a platform dropdown list unselected and don't enter a redirect URI.

Confirm that Permissions > Grant admin consent to openid and offline_access permissions is selected.

Select Register.

## This is how your registered app will look like

![3RegisteredApp](https://github.com/usamabaig202/Today_i_Fixed_.Net/assets/119293426/65594fa2-53ce-441d-89a1-eabf17003ef9)

## Now record the following information.

Server API app Application (client) ID (for example, 41451fa7-82d9-4673-8fa5-69eff5a761fd)

AAD B2C instance (for example, https://example.b2clogin.com/, which includes the trailing slash). The instance is the scheme and host of an Azure B2C app registration, which can be found by opening the Endpoints window from the App registrations page in the Azure portal.

AAD Primary/Publisher/Tenant domain (for example, example.onmicrosoft.com): The domain is available as the Publisher domain in the Branding blade of the Azure portal for the registered app.

## Select Expose an API from the sidebar and follow these steps:

1.	Select Add a scope.
2.	Select Save and continue.
3.	Provide a Scope name (for example, API.Access).	
4.	Provide an Admin consent display name (for example, Access API).
5.	Provide an Admin consent description (for example, Allows the app to access server app API endpoints.).
6.	Confirm that the State is set to Enabled.
7.	Select Add scope.

## Record the following information:

App ID URI GUID (for example, record 41451fa7-82d9-4673-8fa5-69eff5a761fd from https://example.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd)

Scope name (for example, API.Access)

## Register a client app in Azure

Register an AAD B2C app for the Client app:

1.	Navigate to Azure AD B2C in the Azure portal. Select App registrations in the sidebar. Select the New registration button.
2.	Provide a Name for the app (for example, Blazor Client AAD B2C).
3.	For Supported account types, select the multi-tenant option: Accounts in any identity provider or organizational directory (for authenticating users with user flows)
4.	Set the Redirect URI dropdown list to Single-page application (SPA) and provide a redirect URI value of https://localhost/authentication/login-callback. If you know the production redirect URI for the Azure default host (for example, azurewebsites.net) or the custom domain host (for example, example.com), you can also add the production redirect URI at the same time that you're providing the localhost redirect URI. Be sure to include the port number for non-:443 ports in any production redirect URIs that you add.
5.	Confirm that Permissions > Grant admin consent to openid and offline_access permissions is selected.
6.	Select Register.

Record the Client app Application (client) ID (for example, 4369008b-21fa-427c-abaa-9b53bf58e538).

In Authentication > Platform configurations > Single-page application:

Confirm the redirect URI of https://localhost/authentication/login-callback is present.

In the Implicit grant section, ensure that the checkboxes for Access tokens and ID tokens aren't selected. Implicit grant isn't recommended for Blazor apps using MSAL v2.0 or later. For more information, see xref:blazor/security/webassembly/index#use-the-authorization-code-flow-with-pkce.

The remaining defaults for the app are acceptable for this experience.

Select the Save button if you made changes.

## In API permissions from the sidebar:

1.  Select Add a permission followed by My APIs.
2.  Select the Server API app from the Name column (for example, Blazor Server AAD B2C).
3.  Open the API list if it isn't already open.
4.  Enable access to the API (for example, API.Access) with the checkbox.
5.  Select Add permissions.
6.  Select the Grant admin consent for {TENANT NAME} button. Select Yes to confirm.

Return to Azure AD B2C in the Azure portal. Select User flows and use the following guidance: Create a sign-up and sign-in user flow. At a minimum, select Application claims for the sign-up/sign-in user flow and then the Display Name user attribute checkbox to populate the context.User.Identity?.Name/context.User.Identity.Name in the LoginDisplay component (Shared/LoginDisplay.razor).

Record the sign-up and sign-in user flow name created for the app (for example, B2C_1_signupsignin1).

## Create the Blazor app or Chanhes in the existing project:
## For creating new Blazor

Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:
```
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI GUID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -o {PROJECT NAME} -ssp "{SIGN UP OR SIGN IN POLICY}"
```

## Changes in appsetting.json for server project
Add these values from the above recorded values;
```
{
  "AzureAdB2C": {
    "Instance": "https://{TENANT}.b2clogin.com/",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "Domain": "{TENANT DOMAIN}",
    "Scopes": "{DEFAULT SCOPE}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```
## Example
```
{
  "AzureAdB2C": {
    "Instance": "https://example.b2clogin.com/",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "Domain": "example.onmicrosoft.com",
    "Scopes": "API.Access",
    "SignUpSignInPolicyId": "B2C_1_signupsignin1",
  }
}
```

## AddAuthentication in Server project program.cs
```
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(Configuration.GetSection("AzureAdB2C"));


app.UseAuthorization();
```
Add the Authorize attribute on the API
```
[Authorize]
[ApiController]
[Route("[controller]")]
[RequiredScope(RequiredScopesConfigurationKey = "AzureAdB2C:Scopes")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        //your logic
    }
}
```
## wwwroot/appsettings.json on client project
```
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{TENANT DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": false
  }
}
```
## Example
```
{
  "AzureAdB2C": {
    "Authority": "https://example.b2clogin.com/example.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": false
  }
}
```

## Add this code in program.cs for client project
```
builder.Services.AddHttpClient("{PROJECT NAME}.ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddScoped(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{PROJECT NAME}.ServerAPI"));
```
The placeholder {PROJECT NAME} is the project name at solution creation. For example, providing a project name of BlazorSample produces a named xref:System.Net.Http.HttpClient of BlazorSample.ServerAPI.
```
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```
## Access token scopes

The default access token scopes represent the list of access token scopes that are:

Included by default in the sign in request.

Used to provision an access token immediately after authentication.

```
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```
## Example
```
options.ProviderOptions.DefaultAccessTokenScopes.Add(
    "https://example.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd/API.Access");
```

## Run Application
Now run the application, and verify the changes.
