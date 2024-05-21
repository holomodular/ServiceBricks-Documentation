# Domain Objects and Domain-Driven Design
## Overview
Domain-driven design (DDD) is an approach to software development that emphasizes the importance of creating a model of the problem domain to guide the design of the software. 
Domain objects are central to this approach and are at the core of the ServiceBricks platform.

### DomainObject Concepts

* The domain model represents the key concepts, entities, and relationships in the problem domain.
* It is the internal model with provider-specific implementations in each assembly (or uses a shared model).
* Heavily used with self-referencing generic types

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

### DataTransferObject Concepts
* Has a single string property, StorageKey, which is used to identify any domain object through the use of .NET datatype string serialization and delimeters.
* It is the object exposed from a microservice and should always be used with the IApiService

## DomainObjects for the Business Rule Engine
The domain object is at the core the platform and has many uses.

### DomainBroadcast
These domain objects are used to send Service Bus messages so that microservices can communicate with each other.
These classes are used to hold multiple properties or an object, typically a DTO, to be sent with the message.

### DomainEvent
When using Event-Driven Architecture in the platform and this interface signature lets us know.
The functionality of events can be easily removed and changed with the business rule engine.

### DomainProcess
Domain processes are used to denote generic business processes in the platform and this interface signature lets us know.
The functionality of processes can be easily removed and changed with the business rule engine.
Ex. LoginProcess, SendEmailProcess, PlaceOrderProcess, etc...

## DomainObjects for Storage

### AzureDataTableDomainObject
Used to denote a domain model using the AzureDataTables storage provider.
Azure Data Tables also requires objects to have the ITableEntity interface.

```csharp

public interface IAzureDataTablesDomainObject<TDomain> : IDomainObject<TDomain>, ITableEntity
{
}

public interface ITableEntity
{
    string PartitionKey { get; set; }
    string RowKey { get; set; }
    DateTimeOffset? Timestamp { get; set; }
    ETag ETag { get; set; }
}
```

### EntityFrameworkCoreDomainObject
Used to denote a domain model using the EntityFrameworkCore storage provider.
The DomainGetIQueryableDefaults method allows us to setup Include() statements with a query, if needed.
The DomainGetItemFilter method allows us to distinguish one record from another using an object's primary key(s).

```csharp
public interface IEntityFrameworkCoreDomainObject<TDomain> : IDomainObject<TDomain>
{
    IQueryable<TDomain> DomainGetIQueryableDefaults(IQueryable<TDomain> query);

    Expression<Func<TDomain, bool>> DomainGetItemFilter(TDomain obj);
}

```

### MongoDbDomainObject
Used to denote a domain model using the MongoDb storage provider.
The DomainGetItemFilter allows us to distinguish one record from another using an object's primary key(s).

```csharp
public interface IMongoDbDomainObject<TDomain> : IDomainObject<TDomain>
{
    Expression<Func<TDomain, bool>> DomainGetItemFilter(TDomain obj);
}
```
