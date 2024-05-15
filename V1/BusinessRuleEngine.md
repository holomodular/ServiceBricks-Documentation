# Business Rule Engine
## Overview
We have create a simple but powerful business rule engine that allows customization of business logic in the ServiceBricks platform.
By creating **Business Rule** class, you can "hook" into the Event-Driven Architecture (EDA) and customize your microservice, or any existing microservice and customize it however you like.


## Business Rules
Business Rules perform many different functions in the platform and are polymorphic by nature. 
They can perform validations, custom business logic, processes, events, handle Service Bus Broadcasts or anything else needed.

### IBusinessRule
Business Rules define a Priority property that allow ordered execution for expected results. 
You can also specify a property to determine whether the keep processing should an error occur.

It provides a syncronous and asynchronous method and you must implement both. It is acceptable to have one method call the other in the case when sync or async functionality is not available.

```csharp

    /// <summary>
    /// This is a business rule.
    /// </summary>
    public partial interface IBusinessRule
    {
        string Name { get; }

        bool StopOnError { get; }

        int Priority { get; }

        void SetCustomData(Dictionary<string, object> data);

        IResponse ExecuteRule(IBusinessRuleContext context);

        Task<IResponse> ExecuteRuleAsync(IBusinessRuleContext context);
    }

```

When a rule is executed, it is supplied with a context object that is user to pass data along to all business rules registered in the pipeline for that particular type.


## IBusinessRuleRegistry
The IBusinessRuleRegistry interface is used to register Business Rules. 
Business Rules can be added to any type and the registry keeps track of all those associations.

Depending on your needs, you can also add, remove, change and reorder business rules to get the functionality you desire.

### Registering a Business Rule in the Registry
A business rule is associated to a type. 

* If you register a business rule to a domain object type, it will be processed when validating the domain object.
* If you register a business rule to a domain event type, it will be processed when the event is fired.
* If you register a business rule to a domain process type, it will be processed when the process is invoked.
* If you register a business rule to a domain broadcast type, it will be processed when the service bus message is received.

```csharp

BusinessRuleRegistry.Instance.RegisterItem(typeof(ExampleBroadbast), typeof(ExampleBroadcastBusinessRule));

```

## IBusinessRuleService
The Business Rule Service is responsible for executing domain events and processes in the platform.

```csharp

 /// <summary>
 /// Service that executes business rules.
 /// </summary>
 public partial interface IBusinessRuleService
 {
     /// <summary>
     /// Execute a Domain Event
     /// </summary>
     /// <param name="domainEvent"></param>
     /// <returns></returns>
     IResponse ExecuteEvent(IDomainEvent domainEvent);

     /// <summary>
     /// Execute a Domain Event
     /// </summary>
     /// <param name="domainEvent"></param>
     /// <returns></returns>
     Task<IResponse> ExecuteEventAsync(IDomainEvent domainEvent);

     /// <summary>
     /// Execute a Domain Process.
     /// </summary>
     /// <param name="domainProcess"></param>
     /// <returns></returns>
     IResponse ExecuteProcess(IDomainProcess domainProcess);

     /// <summary>
     /// Execute a Domain Process.
     /// </summary>
     /// <param name="domainProcess"></param>
     /// <returns></returns>
     Task<IResponse> ExecuteProcessAsync(IDomainProcess domainProcess);

     /// <summary>
     /// Execute business rules against the context.
     /// </summary>
     /// <param name="context"></param>
     /// <returns></returns>
     IResponse ExecuteRules(IBusinessRuleContext context);

     /// <summary>
     /// Execute business rules against the context.
     /// </summary>
     /// <param name="context"></param>
     /// <returns></returns>
     Task<IResponse> ExecuteRulesAsync(IBusinessRuleContext context);

     /// <summary>
     /// Execute business rules against the context, along with additonal business rules.
     /// </summary>
     /// <param name="context"></param>
     /// <param name="additionalRules"></param>
     /// <returns></returns>
     IResponse ExecuteRules(IBusinessRuleContext context, System.Collections.Generic.IList<IBusinessRule> additionalRules);

     /// <summary>
     /// Execute business rules against the context, along with additonal business rules.
     /// </summary>
     /// <param name="context"></param>
     /// <param name="additionalRules"></param>
     /// <returns></returns>
     Task<IResponse> ExecuteRulesAsync(IBusinessRuleContext context, System.Collections.Generic.IList<IBusinessRule> additionalRules);
 }

```


### Business Rule Context
The BusinessRuleContext class is responsible for hold a reference to the object that is being processed against. 
It is passed to each business rule as each on is evaluated. 
It provides a state dictionary so that you can pass data in-between business rules or add to it along the way.


## Creating Your Own Business Rules
The following is an example of creating a business rule that is bound to a service bus broadcast message.

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
}
```

## Summary
Business Rules can be hooked up to domain objects for validation, domain events for events, domain processes for processing logic and domain broadcasts for service bus communications.
It is a versitile tool used throughout the platform to service many functions.

