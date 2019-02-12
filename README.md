# KK.AspNetCore.BackgroundTasks.Scheduled

This project contains the code for the NuGet Package to get scheduled tasks in an asp.net core application.  

## Usage

> **INFO**: For detailed usage information please have a look in the `KK.AspNetCore.BackgroundTasks.Scheduled.Web.Sample` project.

### Task

You can add multiple tasks in your projekt. The task has to implement the `IScheduledTask` interface.  
The options for the task are provided by the DI system and you can request the options with `IScheduledTaskOptions<SampleTask>` as shown below.  
The `ExecuteAsync` Method will be executed when the task is triggert.

```C#
public class SampleTask : IScheduledTask
{
    public SampleTask(
        IScheduledTaskOptions<SampleTask> options
    )
    {
        this.Options = options;           
    }

    public IScheduledTaskOptions<IScheduledTask> Options { get; }

    public async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        // Stuff to do...
    }
}
```

> **INFO** You shoud use the provided `CancellationToken` in your code to support cancelling of your task.

You have to register the task in your `public void ConfigureServices` of the `Startup.cs`. You can use the provided `AddScheduledTask` Method:

```C#
services.AddScheduledTask<SampleTask>();
```

The Task will be added as a `Singleton` with this method.  
If you does not want this you can also register it by yourself:

```C#
services.AddScoped<IScheduledTask, SampleTask>();
```


### Options

The options are provided as an `IScheduledTaskOptions` of type `YourSampleTask` through the DI system

| Property | Description | Default |
| --- | --- | --- |
| `Schedule` | A CronTab sting which describes the execution schedule | **nothing**, must be set by the user. For the format have a look [here](https://github.com/HangfireIO/Cronos#cron-format) |
| `CronFormat` | A `Cronos.CronFormat` value, this must only be set when you ant to use seconds in your schedule | `Cronos.CronFormat.Standard`, more information [here](https://github.com/HangfireIO/Cronos) |

In your `public void ConfigureServices` of the `Startup.cs` you can add options like so:

```C#
services.AddSingleton<IScheduledTaskOptions<SampleTask>>(
    new ScheduledTaskOptions<SampleTask>
    {
        Schedule = "*/10 * * * * *",
        CronFormat = Cronos.CronFormat.IncludeSeconds
    }
);
```

### SchedulerHost

**After** the registration of all your tasks there is only one thing left to do.  
Register the `SchedulerHostedService` like so:

```C#
services.AddHostedService<SchedulerHostedService>();
```

## NuGet

| Feed | Name | Status |
| --- | --- | --- |
| NuGet.org | [KK.AspNetCore.BackgroundTasks.Scheduled](https://www.nuget.org/packages/KK.AspNetCore.BackgroundTasks.Scheduled/) | [![NuGet Badge](https://img.shields.io/nuget/v/KK.AspNetCore.BackgroundTasks.Scheduled.svg)](https://www.nuget.org/packages/KK.AspNetCore.BackgroundTasks.Scheduled/) |
| Azure DevOps | [KK.AspNetCore.BackgroundTasks.Scheduled](https://feeds.dev.azure.com/kirkone/_apis/public/Packaging/Feeds/70450bc2-9936-4d1b-b153-be005873090e/Packages/8fe0297c-06a6-43c4-a8ba-f897ea6ef494/Badge) | [![Azure DevOps Badge](https://feeds.dev.azure.com/kirkone/_apis/public/Packaging/Feeds/70450bc2-9936-4d1b-b153-be005873090e/Packages/8fe0297c-06a6-43c4-a8ba-f897ea6ef494/Badge)](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_packaging?_a=package&feed=70450bc2-9936-4d1b-b153-be005873090e&package=8fe0297c-06a6-43c4-a8ba-f897ea6ef494&preferRelease=true) |

You can add the package for example with the following `dotnet` command:

```Shell
dotnet add package KK.AspNetCore.BackgroundTasks.Scheduled
```

Pre-releases of this Package are pushed to an internal feed an Azure DevOps. There is no public access to this feeds at the moment.

The build environment for this project is on Azure DevOps and can be found here [dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_build)

#### Build

| Name | Status |
| --- | --- |
| KK.AspNetCore.BackgroundTasks.Scheduled CI | [![Build Status](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_apis/build/status/KK.AspNetCore.BackgroundTasks.Scheduled%20CI?branchName=master)](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_build/latest?definitionId=30&branchName=master) |

#### Release

| Name | Status |
| --- | --- |
| KK.AspNetCore.BackgroundTasks.Scheduled CD | |
| Alpha | [![Alpha](https://vsrm.dev.azure.com/kirkone/_apis/public/Release/badge/b1423fc9-d9b5-4555-8599-ff7a1fdea8f9/2/2)](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_release?view=all&definitionId=2&_a=releases) |
| Beta | [![Beta](https://vsrm.dev.azure.com/kirkone/_apis/public/Release/badge/b1423fc9-d9b5-4555-8599-ff7a1fdea8f9/2/3)](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_release?view=all&definitionId=2&_a=releases) |
| Release | [![Release](https://vsrm.dev.azure.com/kirkone/_apis/public/Release/badge/b1423fc9-d9b5-4555-8599-ff7a1fdea8f9/2/4)](https://dev.azure.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/_release?view=all&definitionId=2&_a=releases) |


## Authors

-   **Kirsten Kluge** - _Initial work_ - [kirkone](https://github.com/kirkone)
-   **paule96** - _Refactoring and understanding awesome interface stuff_ - [paule96](https://github.com/paule96)
-   **TiltonJH** - _Unbelievable knowledge about awesome interface stuff_ - [TiltonJH](https://github.com/TiltonJH)

See also the list of [contributors](https://github.com/kirkone/KK.AspNetCore.BackgroundTasks.Scheduled/graphs/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

-   Inspired by this [Microsoft documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-2.2) and this [Blog post](https://blog.maartenballiauw.be/post/2017/08/01/building-a-scheduled-cache-updater-in-aspnet-core-2.html) repo
