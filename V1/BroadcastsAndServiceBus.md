# Broadcasts and Service Bus
## Overview
A Service Bus is used to send information between microservices asynchronously via inter-process communications.

## Hosting Options
### Intra-Process Service Bus with InMemory Provider
When hosting all your microservices within a single web application or process, we can use the default InMemory service bus provider.
It is derived from the  **TaskQueueHostedService** class using its own processing queue, the same as (BackgroundTask Processing)[https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/BackgroundTasks.md].
This is registered automatically for you when you call the **services.AddServiceBricks()** method in startup.

### Inter-Process Service Bus with Azure Provider
When hosing microservices in their own web application or application domain, we need to use an external service bus provider.
We provide standard support for the Azure Service Bus provider out of the box.


Other providers can also be added, such as Kafka.
Simply follow the pattern defined in the Azure provider as an example.


#### Add Azure Service Bus
To add Azure Service Bus to the pipeline, call the method below based on which option you need.

**Note:** This should be the first line called after the AddServiceBricks() method to ensure registration occurs correctly for other ServiceBricks called in the startup.
```csharp

  services.AddServiceBricks();

  // Use with Basic - Queues
  services.AddServiceBricksServiceBusAzure();

  // Use with Standard/Advanced - Topics and subscriptions
  services.AddServiceBricksServiceBusAzureAdvanced();

  // Add all other ServiceBricks Next - IMPORTANT FOR SERVICE BUS REGISTRATION

```

#### Starting Azure Service Bus
Add the following line to start the service bus option you need.

**Note:** This should be the last line after all other ServiceBricks have been started. 
As soon as the service bus is started, it will begin to process messages and all providers need to be running before this.
```csharp

  // Use with Basic - Queues
  app.StartServiceBricksServiceBusAzure();

  // Use with Standard/Advanced - Topics and subscriptions
  app.StartServiceBricksServiceBusAzureAdvanced();
```

## Broadcast Message
A broadcast message is defined like the following:

```csharp

    /// <summary>
    /// This is an inter-process event raised in the platform. Handled by Service Bus.
    /// </summary>
    public partial class DomainBroadcast : IDomainBroadcast<object>, IDomainBroadcast
    {
        public virtual object DomainObject { get; set; }
    }

    /// <summary>
    /// This is an inter-process event raised in the platform based on a domain object.
    /// </summary>
    /// <typeparam name="TDomainObject"></typeparam>
    public partial class DomainBroadcast<TDomainObject> : IDomainBroadcast<TDomainObject>, IDomainBroadcast
    {
        public virtual TDomainObject DomainObject { get; set; }
    }

```

### Creating your own Broadcast Messages
You can create your own broadcast messages to use in the platform.

Here is an example message that tells all the microservices hello.

```csharp
    public class HelloBroadcast : DomainBroadcast<string>
    {
        public HelloBroadcast()
        {
            DomainObject = "Hello";
        }
    }

```

## Sending a Broadcast Message

```csharp
  var serviceBus = services.GetRequiredService<IServiceBus>();
  var broadcast = new HelloBroadcast();
  _serviceBus.Send(broadcast);

```

## Subscribing to Broadcast Messages
During system startup, we need to register with the service bus to know that you want to receive the broadcast message.
In order to process a broadcast message, you must first create a **Business Rule**.

You create a subscription using the following code:
```csharp

            // ServiceBus Rules
            using (var serviceScope = services.BuildServiceProvider().GetRequiredService<IServiceScopeFactory>().CreateScope())
            {
                var serviceBus = serviceScope.ServiceProvider.GetRequiredService<IServiceBus>();
                serviceBus.Subscribe(typeof(HelloBroadcast), typeof(HelloBroadcastRule));
            }

```

### Business Rule to Process the Broadcast Message

The following is an example business rule that processes the HelloBroadcast message.

You must implement both sync and async logic. Having one method call the other is acceptable in the case where sync or async functionality may not exist or be available.

```csharp
public class HelloBroadcastRule : BusinessRule
{
    private readonly ILogger<HelloBroadcastRule> _logger;

    public HelloBroadcastRule(
        ILoggerFactory loggerFactory)
    {
        _logger = loggerFactory.CreateLogger<HelloBroadcastRule>();
        Priority = PRIORITY_NORMAL; // Set the order it runs in the business rule engine
    }

    /// <summary>
    /// Execute the business rule.
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    public override IResponse ExecuteRule(IBusinessRuleContext context)
    {
        var response = new Response();
        var broadcast = context.Object as HelloBroadcast;

        // Do sync processing here

        return response;
    }

    /// <summary>
    /// Execute the business rule.
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    public override async Task<IResponse> ExecuteRuleAsync(IBusinessRuleContext context)
    {
        var response = new Response();
        var broadcast = context.Object as HelloBroadcast;

        // Do async processing here

        return response;
    }

``




