# Comprehensive Xunit Testing Framework
## Overview
We have built a comprehensive xunit testing framework to ensure that microservices developed on the ServiceBricks platform are working correctly.

## Debug Projects
The assemblies that compose the framework make references to each other using NuGet package references.
This presents an issue when making changes and trying to debug those changes.
You would have to deploy NuGet packages for every change you make just to test those changes.
Not ideal.

Instead, we create debug projects for each assembly. 
Source code files are "linked" instead of added from their parent assemblies they refer do.
All debug projects then reference each other.
This allows you to test and make changes without affect NuGet packages.

## ServiceBricks.Xunit
This testing assembly is available via NuGet with the package name **ServiceBricks.Xunit**.
It provides startup classes and test classes needed to easily begin testing your microservice and objects.

## Xunit Test Projects
### Design
To start, we create a Testing folder under each microservice and it contains all the test projects used for a microservice.
We create projects for each of the referenced .NET libraries we are targeting.
In our case, that means we are creating test projects for .NET 8, .NET 7 and .NET 6 for backwards support.

In order to test all functionality with all supported standard databases, you will need the following test projects.

* ServiceBricks.Microservice.AzureDataTables.Xunit - Azure Data Table database specific
* ServiceBricks.Microservice.Client.Xunit - Client integration tests
* ServiceBricks.Microservice.Cosmos.Xunit - Cosmos database specific
* ServiceBricks.Microservice.InMemory.Xunit - InMemory database specific
* ServiceBricks.Microservice.MongoDb.Xunit - MongoDb database specific
* ServiceBricks.Microservice.Postgres.Xunit - Postgres database specific
* ServiceBricks.Microservice.Sqlite.Xunit - Sqlite database specific
* ServiceBricks.Microservice.SqlServer.Xunit - SqlServer database specific
* ServiceBricks.Microservice.Xunit - Core tests for each domain object implementation
* WebApp - Used to host the service when running the client

### Test Files Needed
For each assembly, you only need the following files:

* Startup[ProviderName].cs - This file starts up system using your provider options
* [DomainObjectName]ApiControllerTest[ProviderName].cs - This implements a generic interface for each domain object. It provides ApiController and ApiService tests.
* [DomainObjectName]ApiClientTest.cs - This implements a generic interface for each domain object. It provides ApiController and ApiService tests.
* MappingTest[ProviderName].cs - This is an AutoMapper test class to ensure your DTO -> Domain object mappings are valid.

### TestManager
This class provides the core wrapper for a domain object.
It provides the interfaces to get a IAPIClients for integration tests and IAPIControllers and IApiServices for unit tests.
You create a TestManager for each domain object you expose and these are placed in the ServiceBricks.Microservice.Xunit assembly.

### Integration Namespace
All test classes have the namespace ServiceBricks.Xunit.Integration.
When we build and run the dotnet test functions during the CI/CD pipeline, we specifically exlude the integration namespace above so that these tests are not run.
If we didn't it would attempt to run all tests connect to all database providers and clients, which we don't want.
Instead, we only change the namespace of the InMember unit tests, so that they are executed and code coverage can be obtained.
Since all providers use the same test files and functions, we can guarentee that the microservice will operate the same expected way for all providers.

## Tests Available
There are 74 unit tests for each provider and 76 client tests by default for each domain object.
You will obtain over 50% unit test code coverage by implementing just the few files needed to confirm your microservice is working correctly.

## Complete
When you are able to run all unit tests for all providers, your microservice is ready to be used in the platform.



