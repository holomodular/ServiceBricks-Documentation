# Request and Response Paradigm
## Overview
The ServiceBricks platform is built using a request and response paradigm.

### Importance of a Response object in a function Signature
As part of our standards and conventions, any function which returns a response object must not throw exceptions.
Also, if a response object is not successful, it must also contain one error message that can be returned to the user.

Following these conventions lets the developer know when to use try/catch blocks, further creating faster, more efficient code.

### Response Object
The response object defines output of a managed function call. 
It provides the success disposition of the overall call and a list of Response Messages that can be displayed to the user.

```csharp

/// <summary>
/// This is a response.
/// </summary>
public partial interface IResponse
{
    /// <summary>
    /// Determine if the response is successful or if an error happened.
    /// </summary>
    bool Success { get; set; }

    /// <summary>
    /// Determine if the response is successful or if an error happened.
    /// </summary>
    bool Error { get; set; }

    /// <summary>
    /// The collection of response messages.
    /// </summary>
    IReadOnlyList<IResponseMessage> Messages { get; }

    /// <summary>
    /// Copy from response object to this instance.
    /// </summary>
    /// <param name="from"></param>
    void CopyFrom(IResponse from);

    /// <summary>
    /// Copy this object to the response object.
    /// </summary>
    /// <param name="to"></param>
    void CopyTo(IResponse to);

    /// <summary>
    /// Add a message to the response. If the message is an error, it will also set the response.Success to false as well.
    /// </summary>
    /// <param name="message"></param>
    void AddMessage(IResponseMessage message);

    /// <summary>
    /// Add a list of messages to the response. If amessage is an error, it will also set the response.Success to false as well.
    /// </summary>
    /// <param name="message"></param>
    void AddMessage(List<IResponseMessage> messages);

    /// <summary>
    /// Get all the messages as a single string.
    /// </summary>
    /// <param name="seperator"></param>
    /// <returns></returns>
    string GetMessage(string seperator);
}

```

### Response Message
The ResponseMessage class provides the user with a severity, message and list of affected fields.

```csharp
    /// <summary>
    /// This is a response message.
    /// </summary>
    public interface IResponseMessage
    {
        /// <summary>
        /// Severity of the message.
        /// </summary>
        ResponseSeverity Severity { get; set; }

        /// <summary>
        /// The message displayed to the user.
        /// </summary>
        string Message { get; set; }

        /// <summary>
        /// The field(s) this messages correlates to.
        /// </summary>
        System.Collections.Generic.IList<string> Fields { get; set; }
    }
```



### Types of Response Objects
We provide the following types of response objects in the platform.

* Response
* ResponseItem - Contains an additional generic property, Item, that contains an object reference.
* ResponseList - Contains an additional generic property, List, that contains a list of object references.
* ResponseCount - Contains an additional integer property, Count, containing a count.
* ResponseAggregateCountList - Currently not used, it was going to be used to map from a ServiceQueryResponse object.
* AjaxResponse - Used for MVC controllers as a result for AJAX methods and JQuery.


## Request Object
A Request object defines no properties at the root level.

```csharp
    /// <summary>
    /// This is a request.
    /// </summary>
    public partial interface IRequest
    {
    }

```

### Types of Request Objects
We provide the following types of response objects in the platform.

* Request
* RequestItem - Contains an additional generic property, Item, that contains an object reference.
* RequestList - Contains an additional generic property, List, that contains a list of object references.
