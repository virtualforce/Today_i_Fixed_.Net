# Problem
How to add multilingual feature in ASP.net core 6.0 C# for different languages(English, Arabic)?

# Environment
Visual Studio 2022, Asp.Net Core 6.0, C#, MVC

# How you fix it
With the help of Multilingual feature, you can translate the content of any View page into your desire language. This can be achieved using the resource files into the resource folder.

# Solution
## Add the below code in Program.cs class.

```
builder.Services.AddLocalization(o => { o.ResourcesPath = "Resources"; });
builder.Services.AddMvc().AddViewLocalization(LanguageViewLocationExpanderFormat.Suffix).AddDataAnnotationsLocalization();

// set the default culture and supported culture list
builder.Services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new List<CultureInfo>
    {
        new CultureInfo("en"),
        new CultureInfo("ar"),
    };
    options.DefaultRequestCulture = new RequestCulture("en");
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;
});
```
## Follow the steps below for adding Resources folder and resource files for each of the view in different controllers.

1- Add the Resources folder in project
2- Add the same folder structure of the Views for example Resources -> Views -> Home and then add all the specified view files which you have in the Home Controller "Index.ar.resx", Index.en.resx
3- Add the below code in the shared layout.

```
@inject Microsoft.Extensions.Options.IOptions<Microsoft.AspNetCore.Builder.RequestLocalizationOptions> locOptions
@inject Microsoft.AspNetCore.Mvc.Localization.IViewLocalizer localizer

@{
    var culture = Context.Features.Get<Microsoft.AspNetCore.Localization.IRequestCultureFeature>();
    var cultureList = locOptions.Value.SupportedUICultures.Select(x => new SelectListItem { Value = x.Name, Text = x.Name }).ToList();
}
```

4- Now add the language change dropdown list to the shared view in the navigation bar section.

```
 <div class="langSwitch">
                    
                    <form asp-action="CultureManagement" asp-controller="Home" method="post">
                        <select name="culture" asp-for="@culture.RequestCulture.UICulture.Name" asp-items="cultureList" onchange="this.form.submit();"></select>
                    </form>
                </div>
```

5- Add the below Action Method in the controller to switch the language.

```
[HttpPost]
        public IActionResult CultureManagement(string culture, string actionName)
        {
            Response.Cookies.Append(CookieRequestCultureProvider.DefaultCookieName, CookieRequestCultureProvider.MakeCookieValue(new RequestCulture(culture)),
                new CookieOptions { Expires = DateTimeOffset.Now.AddDays(30) });

            
            return RedirectToAction(actionName,"Home");
            
        }
```

6- For each View, add the localizer variable and add values to its resource file


```
<h2 class="display-4"> @localizer["Helloworld"]</h2>
```

## Run Application
