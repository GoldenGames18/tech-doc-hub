# 🛡️ Security

[The different types of objects that a controller method can return](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controllerbase?view=aspnetcore-9.0)
###  ✅Validation 
When it comes to securing our routes, C# provides an object validation system based on various annotations that can be applied to our models. It also appears to be enabled by default. [Documentation](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-9.0)

# 🔖 ASP.NET Core Annotations 

This section describes the most common **binding attributes** used in ASP.NET Core controllers.   These attributes specify **where the data comes from** when an API endpoint receives a request.

## 📦 [FromBody] 

Allows securing the objects received in the request body.
```csharp
[HttpPost]
public ActionResult<List<Product>> Get([FromBody] bool discontinuedOnly = false)
{
    List<Product> products = null;

    if (discontinuedOnly)
    {
        products = _productsInMemoryStore.Where(p => p.IsDiscontinued).ToList();
    }
    else
    {
        products = _productsInMemoryStore;
    }

    return products;
}
```

## 🛣️ [FromRoute] 

Allows specifying/verifying what is received as a parameter in our route.
```C#
[HttpPut("{id:guid}")]
public ActionResult<UserDto> Put ([FromRoute] Guild id)
```

## [FromHeader]

Allows specifying what is received in the header of our route.
```C#
[HttpPut]
public ActionResult<UserDto> Put ([FromHeader] Guild id)
```

## [FromQuery]

Allows specifying the parameters you want to include in your request.
```C#
[HttpGet]
public async Task<IActionResult> GetAllBooks(string shelfID,[FromQuery] string ID, [FromQuery] string Name)
```

## [Consumes] | [Produces]

Allows specifying the format of data that will be sent to or returned by your application — for example, **JSON** or **XML**.

When applied at the controller level, all methods in the controller will only accept or produce the defined format.

```C#
[ApiController]
[Route("[controller]")]
[Consumes("application/json")]
[Produces(MediaTypeNames.Application.Json)]
public class WeatherForecastController : ControllerBase
{
}
```

You can also apply it to a single endpoint:
```C#
 [HttpGet(Name = "GetWeatherForecast")]
 [Consumes("application/json")]
 public IEnumerable<WeatherForecast> Get()
```


