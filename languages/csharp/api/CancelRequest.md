# ✋🏻Cancel Request

A `CancellationToken` can be passed to our controller method. Its purpose is simply to interrupt the request if, for example, the client disconnects or directly cancels a request. This token must be passed to the service and repository to properly stop any database requests (UseDbContextFactory), for instance.

```C#
[HttpGet(Name = "GetWeatherForecast")]
[Consumes("application/json")]
public IEnumerable<WeatherForecast> Get(CancellationToken cancellationToken)
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
```

⚠️ It is even recommended to always include a CancellationToken for every asynchronous method.