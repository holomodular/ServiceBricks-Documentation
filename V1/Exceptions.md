# Exceptions
We only define one exception in the ServiceBricks platform called **BusinessException**. 
It is the root exception from which others should be derived from, should you create others.

The BusinessException class incorporates Response objects as well as ResponseMessages so it can be easily converted and used within business rule engine and with output responses.

```csharp
    /// <summary>
    /// This is the base exception for all managed exceptions in the platform.
    /// Developers should expect that the "Message" property will be
    /// displayed to the user and must not contain any sensitive information.
    /// </summary>
    public partial class BusinessException : Exception, IBusinessException
    {
    }

```

## Sensitive Information in the Message Property

The Message property of the BusinessException will be returned to the user as an error message.
Care should be taken to make sure that you only use human-readable and sanitized responses.
Existing code uses the BusinessException also uses the LocalizationResource for pre-defined error strings.


# Exception Middleware

We recommend the use of exception middleware in all web applications to trap any un-caught exceptions during the host's lifetime.
All of our examples use this and it also responsible for handling Classic vs Modern REST API Design by returning either a problemdetails object or a response object to the APIs.

When registering a middleware component, make sure it is registered in the pipline by calling: 

```csharp
  app.UseMiddleware<ExceptionMiddleware>();
```



