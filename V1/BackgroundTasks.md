# Background Tasks and Timers
## Overview
The ServiceBricks platform facilitates background processing using Hosted Services.

Hosted services are added to the .NET pipeline by using the following line of code:

```csharp
services.AddHostedService<MyHostedService>();
```

### Issues with Long Running Tasks
There are several issues with long running tasks. 

Since these are hosted inside of a web application, the service can be shutdown at any time.
This means tasks need to be fault-tolerant and expect failures in the middle of processing.

Since this is a queue, all background tasks in the queue are blocked from running until all previous ones complete.
This can be mitigated by spawning tasks threads but then you may run into order issues or resource starvation, defeating its intended purpose.

There are other tools and libraries available that could help allieviate these issues, such as HangFire.
This would have to be implemented as another provider and option to use in the framework.

### TaskQueueHostedService
This is the primary background processing queue used in the ServiceBricks platform.
You can queue tasks to run on the background task and they will run in order, one at a time.
When the service is shutdown, all processing stops and any tasks in the queue are lost.

Adding a background task to the task queue is pretty straightforward.
You are going to Queue a WorkDetail class (background task) and it will be processed when next available.

```csharp

  var backgroundTaskQueue = service.GetRequiredService<ITaskQueue>();
  backgroundTaskQueue.Queue(new WorkDetail());

```

### Defining a Background Task
With the help of generics, we define a request class that accepts properties we can pass to the background task.
This provides us a signature we can use to find the appropriate worker later when processing.

This is done by defining the worker detail class.
```csharp

        public class WorkDetail : ITaskDetail<WorkDetail, Worker>
        {
            public WorkDetail(int property1, string property2)
            {
                Property1 = property1;
                Property2 = property2;
            }

            public int Property1 {get;set;}
            public string Property2 {get;set;}

        }

```

Once the WorkDetail class is created, we define the Worker class which does the processing.

```csharp

 public class Worker : ITaskWork<WorkDetail, Worker>
 {
     private readonly ILogger<Worker> _logger;

     public Worker(
         ILogger<Worker> logger)
     {
         _logger = logger;
     }

     public async Task DoWork(WorkDetail detail, CancellationToken cancellationToken)
     {
         // Do your processing here
     }
 }

```

Finally, you will need to register your worker with dependency injection.
This is done with the following code:

```csharp

  services.AddScoped<Worker>();

```



### Background Timers
We provide a background timer implementation with the **TaskTimerHostedService** class. The Interval determines how long apart each tick is. 
The DueTime is the inital wait period when the service starts before it will start processing.

An example timer is defined below. Note that we pass the WorkDetail and Worker classes created above as the processing that will occur when the timer tick is executed.

```csharp
    public class ExampleTimer : TaskTimerHostedService<WorkDetail, Worker>
    {
        public ExampleTimer(
            IServiceProvider serviceProvider,
            ILoggerFactory logger) : base(serviceProvider, logger)
        {
        }

        public override TimeSpan TimerTickInterval
        {
            get { return TimeSpan.FromMinutes(30); }
        }

        public override TimeSpan TimerDueTime
        {
            get { return TimeSpan.FromMinutes(30); }
        }

        public override ITaskDetail<WorkDetail, Worker> TaskDetail
        {
            get { return new WorkDetail(1,"test"); }
        }

        public override bool TimerTickShouldProcessRun()
        {
            return ApplicationBuilderExtensions.ModuleStarted && !IsCurrentlyRunning;
        }
    }

```

The TimerTickShouldProcessRun() function checks to make sure that the microservice is first started and also that it is currently not executing in the background.
You can add other logic here to determine whether the task should run or not for each tick.

Finally, you will need to register this hosted service into the .NET pipeline by calling the following line:

```csharp

services.AddHostedService<ExampleTimer>();

```
