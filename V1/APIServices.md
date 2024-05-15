# The Application Programming Interface (API)
At the heart of the ServiceBricks platform is the application programming interface (API).
APIs are sets of rules and protocols that allow software applications to communicate with each other. 
They define how different software components can exchange information.

## Functionality

### Communication Contract
APIs specify a communication contract how data should be sent and received between components.

### Accessing Functionality and Data
Developers use APIs to access specific functionality or data from third-party code.

### Microservices Usage
Microservices expose their functionality via APIs so that other microservices can use them when needed.


# IApiService
The IApiService interface defines the contract used by both the server and clients to exchange information.
By implementing the IApiService interface, we get access to all our pre-defined CRUD methods, including a query operation with the help of ServiceQuery.

```csharp

/// <summary>
/// This is the API service for a Dto object.
/// </summary>
/// <typeparam name="TDto"></typeparam>
public interface IApiService<TDto> : IApiService
    where TDto : class
{
    IResponseItem<TDto> Get(string storageKey);

    Task<IResponseItem<TDto>> GetAsync(string storageKey);

    IResponseItem<TDto> Update(TDto dto);

    Task<IResponseItem<TDto>> UpdateAsync(TDto dto);

    IResponseItem<TDto> Create(TDto dto);

    Task<IResponseItem<TDto>> CreateAsync(TDto dto);

    IResponse Delete(string storageKey);

    Task<IResponse> DeleteAsync(string storageKey);

    IResponseItem<ServiceQueryResponse<TDto>> Query(ServiceQueryRequest request);

    Task<IResponseItem<ServiceQueryResponse<TDto>>> QueryAsync(ServiceQueryRequest request);
}
```

## Server
A server is a publisher of an API. It accepts a request, processes it and returns a response.
Whenever a developer accesses a microservice or an object inside of a microservice, they should always use the **IApiService interface**.
You should not use a DomainRepository or a StorageRepository directly to manipulate objects.
This is because we define events in the IApiService that may not be fired if it is not used.

### Registering IApiService with Dependency Injection
IApiService interfaces are registered for you when you call the **services.AddServiceBricks[ServiceName]StorageProvider()** method.
It is the storage provider's resposibility to call its dependent core library during its startup routine.

For instance, when starting the Logging Microservice, you simply call:
```csharp
services.AddServiceBricksLoggingInMemory(Configuration);
```
The InMemory provider assembly will make the call to register the IApiServices it needs, for you.

## Client
A client is a consumer of an API. A client can be another web application, a windows service, a console app, a device or a web browser.
In the ServiceBricks platform, a client uses a HttpClient to access a REST-based API.

Whenever a developer needs to call a microservice outside of their host process, they should always use the **IApiClient interface**.

```csharp
    public interface IApiClient<TDto> : IApiService<TDto>
        where TDto : class
    {
        string ApiResource { get; set; }
    }
```

### Registering IApiClient with Dependency Injection
In order to register IApiClient interfaces for another microservice, you need to do two things:

1) Add a reference to the microservice's core NuGet library, such as ServiceBricks.Logging or ServiceBricks.Cache.
2) Call the ServiceCollectionsExtensions method for **services.AddServiceBricks[ServiceName]Client()** method. This will register the interfaces with dependency injection.

For example, this will add the Logging Microservice IApiClient interfaces:
```csharp
services.AddServiceBricksLoggingClient(Configuration);
```

# Controllers
A controller is an abstraction layer used to expose the API Service on a server.
It is responsible for holding attributes, such as for routing requests, authorize attributes for authorization or for other needs.

In the ServiceBricks platform, this is accessed by the **IApiController inteface***.

## IApiController
The IApiController interface provides the same methods as the IApiService interface except that the output for each method uses an ActionResult or Task<ActionResult> as the response.
This allows us to do some translation when sending and receiving requests, namely allowing the option of using Classic vs Modern REST API Design modes.
After inspecting the services configuration, it will return either standard serialized data objects or response objects containing the data.
It is up to the developer to choose their preference.

```csharp
public interface IApiController<TDto>
{
    ActionResult Create([FromBody] TDto dto);

    Task<ActionResult> CreateAsync([FromBody] TDto dto);

    ActionResult Update([FromBody] TDto dto);

    Task<ActionResult> UpdateAsync([FromBody] TDto dto);

    ActionResult Delete([FromQuery] string storageKey);

    Task<ActionResult> DeleteAsync([FromQuery] string storageKey);

    ActionResult Get([FromQuery] string storageKey);

    Task<ActionResult> GetAsync([FromQuery] string storageKey);

    ActionResult Query([FromBody] ServiceQueryRequest request);

    Task<ActionResult> QueryAsync([FromBody] ServiceQueryRequest request);

    ActionResult GetErrorResponse(IResponse response);
}
```

## Security Policies
The ServiceBricks platform exposes two security policies by default.

 * Admin Policy
 * User Policy

When implementing API Controllers for your microservice, you have the choice of using three base ApiController types.

* ApiController - This contains no security and should rarely be used
* AdminPolicyApiController - This contains an authorize attribute requiring the user to have the ADMIN role.
* UserPolicyApiController - This contains an authorize attribute requiring the user to have the USER role.


## Defining Security Policies
You define policies and their rules in your web application.

The following code overrides all security policies and allows anyone to access the API.
```csharp
        // Add Authorization
        services.AddAuthorization(options =>
        {
            //Add Built-in Security Policies
            options.AddPolicy(ServiceBricksConstants.SECURITY_POLICY_ADMIN, policy =>
                policy.RequireAssertion(context => true));

            options.AddPolicy(ServiceBricksConstants.SECURITY_POLICY_USER, policy =>
                policy.RequireAssertion(context => true));
        });
```
