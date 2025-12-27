# 🌄Background service

BackgroundServices run, as their name indicates, at application startup in dedicated threads. Developers decide their specific tasks. They pair effectively with Channels for secure, asynchronous operations in applications.

## Implementation


### ⚠️ Prerequisites ⚠️

```C#
Microsoft.Extensions.Hosting
```

Working with Dependency Injection

```C#
public class Program
{
    public static async Task Main(string[] args)
    {
        using IHost host = Host.CreateDefaultBuilder(args)
            .ConfigureServices(services =>
            {
                services.AddHostedService<ExampleBackgroundService>();
            })
            .Build();

        await host.RunAsync();
    }
}
```

### BackgroundService

A BackgroundService is a background task that runs throughout the application's lifecycle. A crashing BackgroundService can cause the entire application to shut down.

```C#
public class ExampleBackgroundService:  BackgroundService
{
    private readonly ILogger<ExampleBackgroundService> _logger;

    public ExampleBackgroundService(ILogger<ExampleBackgroundService> logger)
    {
        _logger = logger;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        _logger.LogInformation("Le service en arrière-plan démarre.");

        while (!stoppingToken.IsCancellationRequested)
        {
            _logger.LogInformation("Service en cours d'exécution à: {time}", DateTimeOffset.Now);
            await Task.Delay(1000, stoppingToken); // pause 1s
        }

        _logger.LogInformation("Le service en arrière-plan s'arrête.");
    }

    public override async Task StopAsync(CancellationToken stoppingToken)
    {
        _logger.LogError("Le service en arrière-plan s'arrête.");
    }
}
```

### IHostedService

IHostedService is used for a service that allows initializing certain data. Unlike BackgroundService, it can be stopped without causing your application to crash.


```csharp
public class MyBackgroundTask : IHostedService 
{

  public Task StartAsync(CancellationToken cancellationToken) {
    // Implementation logic here

    return Task.CompletedTask; 
  }

  public Task StopAsync(CancellationToken cancellationToken) {
    // Clean up any resources

    return Task.CompletedTask;
  }

}
```

