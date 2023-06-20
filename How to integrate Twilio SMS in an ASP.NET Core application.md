# How to integrate Twilio SMS in an ASP.NET Core application.

# Problem
We generally use OTP service to add an extra layer of security to our application. To deliver these OTP's to the user we can use an email or SMS delivery system. 

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
Using the Twilio's Programmable Messaging API.

# Solution
First we have to install the twilio package by running the following command in the Package Manager Console:
```
    Install-Package Twilio
```

Then we have to create a Twilio account so we can get the Account SID and an Auth Token. 

Afterwards we have to create a Twilio client by selection the option from the menu.

Then coming back to our Visiual Studio, we can send an SMS to our device using the following function:
```


    public void SendSms(string fromPhoneNumber, string twilioProvidedNumber, string message)
    {
        TwilioClient.Init(AccountSid, AuthToken);

        var smsMessage = MessageResource.Create(
            body: message,
            from: new Twilio.Types.PhoneNumber(twilioProvidedNumber),
            to: new Twilio.Types.PhoneNumber(toPhoneNumber)
        );
    }
```
Here the AccountSID and an AuthToken are the ones we got when we created our Twilio account. 
