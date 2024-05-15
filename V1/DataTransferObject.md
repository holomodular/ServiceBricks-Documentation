# Data Transfer Object (DTO)
In the context of microservices, a Data Transfer Object (DTO) is a lightweight object that serves as a data structure for transporting information between different parts of an application. 

## Purpose of DTOs

### Isolation and Modularity

Microservices are modular and isolated components that work together to form an application. Each microservice may have its own data model.

### Data Exchange
To exchange data between microservices, we create shared DTOs. These DTOs contain relevant fields and do not include business logic.

### REST APIs
When microservices communicate with each other through REST APIs, they use DTOs to pass data.

### Domain Models vs. DTOs
In microservices, domain models (representing the application domain) are separated from the data models because we don’t want to expose the complexity of the domain to clients.

# The Solution for Storage Agnostic APIs

## IDataTransferObject
In order for us to support multiple database engines, we must first overcome two major hurdles.

* Identifying an object in both a relational (SQL) and document (NoSQL) database engine.

Object are typically identified with a primary key. There may be one or many keys in order to identify it, as well as each using different data types.

* Querying for objects in both a relational (SQL) and document (NoSQL) database engine.

This is the reason we built [ServiceQuery](https://github.com/holomodular/ServiceQuery) and have included it into the ServiceBricks platform!

```csharp
    using ServiceBricks;

    public interface IDataTransferObject
    {
        string StorageKey { get; set; }
    }

    public interface IDataTransferObject<TDto> : IDataTransferObject
    {
    }
```

## Using the StorageKey property
Our solution to this problem is to map all primary keys down to a single string value called the **StorageKey**.
We can do this because all .NET datatypes can be serialized down to a string value. 
Then all we need to do is use a suitable delimeter to separate each value when combining them or splitting them.
AutoMapper is the perfect tool to help accomplish this and the reason for its inclusion into the platform.

