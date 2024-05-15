# Modules and Mappings

## Modules
Modules are used to identify assemblies in the ServiceBricks platform. Inside of your library, create a new class and inherit the IModule interface.

```charp
    using ServiceBricks;

    public class ExampleModule : IModule
    {
        public List<Assembly> AutomapperAssemblies { get; }
        public List<Assembly> ViewAssemblies { get; }
        public List<IModule> DependentModules { get; }
    }
```

Each assembly in the ServiceBricks platform should create a module class.

## Registering a Module
In order to register a module, you need to add a reference into the ModuleRegistry during system startup.
This is typically done in a Extensions/ServiceCollectionExtensions.cs file during the AddServiceBricks...() method call.

```csharp
    using ServiceBricks;

    public static class ServiceCollectionExtensions
    {
        public static IServiceCollection AddServiceBricksExample(this IServiceCollection services, IConfiguration configuration)
        {
            // Add to module registry
            ModuleRegistry.Instance.RegisterItem(typeof(ExampleModule), new ExampleModule());
        }
    }
```

**NOTE: This call must be done prior to the call the services.AddServiceBricksComplete().** 
Make sure to following standard startup procedures.

## AutoMapper Registration

Using the IModule interface, we let the platform know that our assembly contains AutoMapper Profiles used for object mapping.
During the **services.AddServiceBricksComplete()** method, the platform will iterate through all modules and request their list of AutomapperAssemblies properties.
It will then get a unique list and supply them to the AutoMapper configuration. In a future release, AutoMapper will later be abstracted as another provider that can be used, 
rather than built into the core library.

To let the platform know that your assembly contains AutoMapper Profiles, simply set the property in the constructor as in the follow example:

```charp
    using ServiceBricks;

    public class ExampleModule : IModule
    {
        public ExampleModule()
        {
            AutomapperAssemblies = new List<Assembly>()
            {
                typeof(ExampleModule).Assembly
            };
        }

        public List<Assembly> AutomapperAssemblies { get; }
        public List<Assembly> ViewAssemblies { get; }
        public List<IModule> DependentModules { get; }
    }
```

## DependentModules

This property is not currently being used by the core framework and have been added for future use.

## The Plan For It
This will hold a reference to all assemblies that your library references (as in your project files references). 
In a future version, the platform will be able to find all modules, instantiate them, walk the dependency tree and automatically call the Add() and Start() methods for all modules.
This way the ServiceBricks platform can be added and started with two lines of code.

## ViewAssemblies

This property is not currently being used by the core framework and have been added for future use.

## The Plan For It
This will hold a reference to all assemblies that contain Views so that they can be automatically loaded for rendering in the MVC pipeline.
