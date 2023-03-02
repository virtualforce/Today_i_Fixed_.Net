# How to send FCM Notification through Asp.Net core application

# Problem
Sending Notifications is a crucial part of our application if we are creating a backend for mobile app.  

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
We can use Firebase Cloud Messaging for this purpose.

# Solution
First we have to install the FirebaseAdmin package from our nuget package manager.
This package is created and maintained by google itself.

Then from our firebase account we have to install the firebase cloud messaging key file that contains all the json key value pairs we require to interact with our FCM account through our application.

After getting the file we can store it in our project directly were our appsettings.json file reside.

Then in our startup we have to add the following code.
```
var defaultApp = FirebaseApp.Create(new AppOptions()
{
    Credential = GoogleCredential.FromFile(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "fcm-key.json")),
});
```
Here "fcm-key.json" is the name of the file we donwloaded.

Now we will create a service that can be utilized in different parts of the application to send notifications to the frontend.
```
public async Task<string> SendUserNotification(string title, string body, string deviceId)
{
    var message = new Message()
    {
        Notification = new Notification
        {
            Title = title,
            Body  body
        },
        Token = deviceId
    };
    return await SendNotification(message);
}
```
Here the SendNotification function is provided to us by FirebaseAdmin package.

The deviceId is the id of the device to whom we want to send the notificationt to.
