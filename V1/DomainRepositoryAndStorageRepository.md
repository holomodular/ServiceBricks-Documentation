# Domain Repository and Storage Repository
## Overview
At the heart of the Domain-Driven Design (DDD) used for the platform is the IDomainObject and IRepository interface.
These interfaces provide the base implementation of our design and are used to extend the functionality to our multiple storage providers.

## IDomainObject
The IDomainObject interface is only a signature with no explicit properties or methods. This allows us to find, use and extend functionality for our known types.
You will also note the use of self-referencing generic types.
This is a designed feature of the platform and also helps for refection in certain situations.

```csharp
    /// <summary>
    /// This is the base interface that all domain objects inherit from.
    /// </summary>
    public interface IDomainObject
    {
    }

    /// <summary>
    /// This is the base interface that all domain objects inherit from.
    /// </summary>
    /// <typeparam name="T"></typeparam>
    public interface IDomainObject<T> : IDomainObject
    {
    }
```

## IRepository
The IRepository interface provides the standard methods that our domain objects will expose for basic data manipulation.
Note the use of response objects for each of the method signatures.
When a response object is returned, we know that the method has a try/catch block to handle exceptions and will return a success disposition.
This makes writing wrapper code pretty straight forward.

```csharp
public partial interface IRepository<TDomain>
    where TDomain : class
{
    IResponse Create(TDomain model);

    Task<IResponse> CreateAsync(TDomain model);

    IResponse Update(TDomain model);

    Task<IResponse> UpdateAsync(TDomain model);

    IResponse Delete(TDomain model);

    Task<IResponse> DeleteAsync(TDomain model);

    IResponseItem<TDomain> Get(TDomain model);

    Task<IResponseItem<TDomain>> GetAsync(TDomain model);

    IResponseItem<ServiceQueryResponse<TDomain>> Query(ServiceQueryRequest request);

    Task<IResponseItem<ServiceQueryResponse<TDomain>>> QueryAsync(ServiceQueryRequest request);
}
```

## IStorageRepository
For each database engine we support, it has to expose a StorageRepository that allows us to perform these basic data access methods.
Every domain object using that engine will use, they will be accessed via the same StorageRepository.
This layer does not have any events or extensibility, ensuring all object implementations are the same.
Generics are heavily used so that the programmer does not have any to implement anything other than the class definition.
```chsharp
    /// <summary>
    /// This is the storage repository interface.
    /// </summary>
    /// <typeparam name="TDomain"></typeparam>
    public interface IStorageRepository<TDomain> : IDomainRepository<TDomain>
        where TDomain : class
    {
    }
```

## IDomainRepository
A DomainRepository is the extensible, business logic portion of the IReposiory interface.
This layer contains the Event-Driven Architecture (EVA) for the platform.
It is responsible for firing the validation, before and after events for each domain object, as well as calling the IStorageRepository for actual data manipulation.

* DomainCreateBefore
* DomainCreateAfter
* DomainGetBefore
* DomainGetAfter
* DomainUpdateBefore
* DomainUpdateAfter
* DomainDeleteBefore
* DomainDeleteAfter
* DomainQueryBefore
* DomainQueryAfter

```csharp
    public partial interface IDomainRepository<TDomainObject> : IRepository<TDomainObject>
        where TDomainObject : class
    {
        /// <summary>
        /// Get the underlying storage repository.
        /// </summary>
        /// <returns></returns>
        IStorageRepository<TDomainObject> GetStorageRepository();
    }
```

### IApiService
The IApiService layer only calls the IDomainRepository.
The IApiService is also responsible for Event-Driven Architecture (EVA) in the platform by calling its own Before and After methods.

* ApiCreateBefore
* ApiCreateAfter
* ApiGetBefore
* ApiGetAfter
* ApiUpdateBefore
* ApiUpdateAfter
* ApiDeleteBefore
* ApiDeleteAfter
* ApiQueryBefore
* ApiQueryAfter

## Diagram
![Event-Driven Architecture](https://github.com/holomodular/ServiceBricks-Documentation/blob/main/V1/EventDrivenArchitecture.png)  
