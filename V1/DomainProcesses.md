# Domain Processes
## Overview
A Domain Process is nearly identical to a Domain Broadcasts and a Domain Events but differs in its usage.
A Process defines a generalized set of steps for wrapping a whole process together.

An example of a process would be registering a user into your security system of your website.
1) Create a user account for the user based on their email
2) Send the user a confirmation email

This outlines the basic steps for a "Register User" process.
You then define a business rule (or multiple) that will be executed when this process is invoked.
You can now invoke this process by using the IBusinessRuleService.

```csharp
    var businessRuleService = services.GetRequiredService<IBusinessRuleService>();
    var process = new RegisterUserProcess(email);
    var response = businessRuleService.ExecuteProcess(process);

```

You will need to associate your business rule(s) to the Process by registering it with the BusinessRuleRegistry.
See our Business Rule Engine section for more information.

### Extending Existing Processes
Similar to broadcasts and event, you can extend functionality of any existing process.
You can remove an existing implementation by removing it from the BusinessRuleRegistry.
You can also add steps and change their order (using priority).
This allows you change existing broadcasts, events and processes of other pre-compiled microservices to do what you need!
