# How to send Data Notification using Firebase Admin

# Problem
Like FCM user notification, data notification is also sent to the frontend device but it is not visible to the user. 

It can be used to trigger some event to store some information internally without letting the user know about the notification.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
Firebase Cloud messaging provides us with the capability to send user notification to the frontend device.

# Solution
Same as in FCM user notification, first we have to install the FirebaseAdmin package from our nuget package manager.
This package is created and maintained by google itself.

Then from our firebase account we have to install the firebase cloud messaging key file that contains all the json key value pairs we require to interact with our FCM account through our application.

After getting the file we can store it in our project directly were our appsettings.json file resides.

Then in our startup we have to add the following code.
```
var defaultApp = FirebaseApp.Create(new AppOptions()
{
    Credential = GoogleCredential.FromFile(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "fcm-key.json")),
});
```

Now we will create a service that can be utilized in different parts of the application to send data notifications to the frontend.
We can send anything in the data notification in the key:value pair form.
```
public async Task<string> SendDataNotification(string name, string email, string deviceId)
{
    var message = new Message()
    {
        Data = new Dictionary<string, string>()
        {
            {"name", name},
            {"email",  email}
        },
        Token = deviceId
    };
    return await SendNotification(message);
}
```
The SendNotification function is provided to us by FirebaseAdmin package.

Here we are sending name and email to the frontend using data notification.

The deviceId is the id of the device to whom we want to send the notificationt to.
