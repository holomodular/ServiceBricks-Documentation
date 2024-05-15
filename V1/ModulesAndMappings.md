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

Each assembly in the ServiceBricks platform should create a module.

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
In a future version, the platform will be able to walk the dependency tree to automatically call Add() and Start() for all modules.
This way the ServiceBricks platform can be added and started with two lines of code.

## ViewAssemblies

This property is not currently being used by the core framework and have been added for future use.

## The Plan For It
This will hold a reference to all assemblies that contain Views so that they can be automatically loaded for rendering in the MVC pipeline.
