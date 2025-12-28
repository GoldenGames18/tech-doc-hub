# 🧵Serilog

Allows logging different information from our application and is much lighter and more modular than log4net.

## ⚙️Configuration

### { } With the configuration file of our API.


![Image](../../resources/Serilog.png)


```Csharp
builder.Host.UseSerilog((hostingContext, loggerConfiguration) =>
{
    loggerConfiguration.ReadFrom.Configuration(hostingContext.Configuration);
});
```

To configure Serilog from the JSON file, its structure must look like this:

```json
"Serilog" : {
    "MinimumLevel": {
      "Default": "Debug",
      "Override": {
        "Microsoft": "Information",
        "System": "Information"
      }
    },
    "Using": ["Serilog.Sinks.Console"],
    "WriteTo": [
      {
        "Name": "Console",
        "Args": {
          "outputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level:u3}]  {Message}  :  {SourceContext} {NewLine}{Exception}"
        }
      }
    ]
  }
```

The advantage of Serilog is precisely its modularity and the fact that many developers add modules of all kinds, which opens the way to modularity. Compared to log4net, which forces the use of certain complex modules and requires the creation of a XAML file for its configuration.

## 🪵How to log

```C#
public class WeatherForecastController : ControllerBase
{        
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };

    private readonly ILogger<WeatherForecastController> _logger;

    public WeatherForecastController(ILogger<WeatherForecastController> logger)
    {
        _logger = logger;
    }

    [HttpGet(Name = "GetWeatherForecast")]
    [Consumes("application/json")]
    public IEnumerable<WeatherForecast> Get()
    {
        _logger.LogInformation("Get WeatherForecast");
        return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                TemperatureC = Random.Shared.Next(-20, 55),
                Summary = Summaries[Random.Shared.Next(Summaries.Length)]
            })
            .ToArray();
    }
}
```


