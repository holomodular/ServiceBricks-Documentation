# ServiceQuery - Dynamically Query Data over REST APIs
[ServiceQuery](https://github.com/holomodular/ServiceQuery) is a universal data querying API for polyglot data access.
Similar to how OData and GraphQL work, it leverages the power of an expressions builder and a simple model that is capable of serializing query instructions over service boundaries. 
It supports numerous popular relational (SQL) and document (NoSQL) database engines that expose an IQueryable interface. 

ServiceQuery builds LINQ expressions using individually mapped functions and parsed data that eliminates injection attacks, so querying your data is safe and secure. 
It  provides clients and front end applications unprecedented queryability using a standardized endpoint supporting polyglot data access.

<img src="https://github.com/holomodular/ServiceQuery/blob/main/Logo.png" title="ServiceQuery Logo" width="250"/>

## Developed by HoloModular
Along with ServiceBricks, we created ServiceQuery in 2002 to support our goal of a polyglot architecture.

# Use with ServiceBricks
ServiceQuery provides the foundation for the **Query and QueryAsync** methods in the ServiceBricks platform.
It is a powerful tool that also allows us to build queries seamlessly and transparently across database engines.
It is also used in complex situations in ServiceBricks processing, where existing tools and technology just don't work.

# Use Independently
ServiceQuery is a small but powerful library with no external references that works independently of any database engine using the IQueryable interface. 
During early development, we pulled the ServiceQuery code out of the ServiceBricks platform and into its own standalone library.
We knew this was going to be a great addition to the open-source community and we hope you have found it useful.
And it also works with just a bunch of objects in a list!
