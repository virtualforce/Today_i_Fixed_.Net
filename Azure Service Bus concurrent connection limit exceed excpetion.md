# Problem
Azure Service Bus client can open upto 5000 concurrent connection and a single client. If you need to send/receive a large number of messages to/from service bus connection, you will get `ServiceBusException`


# Environment
Visual Studio 2022, .Net 6, C#, Azure Service Bus Client

# How you fix it
 In order to send more than 5000 message asyncronously, we need to create more instances of Service Bus client. This means that we need to change the lifetime of service

To learn more about service bus `Messaging Quota` [please follow this link] (https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-quotas)

To learn more about Singleton vs Scoped, [please follow this link] (https://www.dofactory.com/code-examples/csharp/service-lifetimes)

# Solution
- Go to ```Startup.cs``` file

- Update service registration lifetime from ```Singleton``` to ```Scoped```

```
	#region Service Bus
        
        builder.Services.AddScoped(new ServiceBusClient(//connectionString));
        builder.Services.TryAddScoped<IServiceBusMessageSender, ServiceBusMessageSender>();

        #endregion
```                
                        
