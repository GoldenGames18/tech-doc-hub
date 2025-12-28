# 🏷️ Version control

The ability to version its API allows to easily create new endpoints without having to overwrite the old ones which must remain compatible for example if clients must update their software due to this change

## 🖋️Native

```C#
[ApiController]
[Route("v1/[controller]")]
public class MyController : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok("Version 1");
    }
}
```

## 📚 With lib

Names of the libraries. [Documentation](https://dev.to/iamrule/comprehensive-guide-to-api-versioning-in-net-8-1i9j)

```C#
Asp.Versioning.Http
Asp.Versioning.Mvc.ApiExplorer
```

Modifications to be made at the controller level.

```C#
namespace MyApp.Controllers.v1
   {
       [ApiVersion("1.0")]
       [Route("api/v{version:apiVersion}/[controller]")]
       [ApiController]
       public class WorkoutsController : ControllerBase
       {
           [MapToApiVersion("1.0")]
           [HttpGet("{id}")]
           public IActionResult GetV1(int id)
           {
               return Ok(new { Message = "This is version 1.0" });
           }
       }
   }

   namespace MyApp.Controllers.v2
   {
       [ApiVersion("2.0")]
       [Route("api/v{version:apiVersion}/[controller]")]
       [ApiController]
       public class WorkoutsController : ControllerBase
       {
           [MapToApiVersion("2.0")]
           [HttpGet("{id}")]
           public IActionResult GetV2(int id)
           {
               return Ok(new { Message = "This is version 2.0", NewField = "New data" });
           }
       }
   }
```

For this modification to work, it is first necessary to add some information at the Program.cs file level

```C#
var builder = WebApplication.CreateBuilder(args);

   builder.Services.AddApiVersioning(options =>
   {
       options.DefaultApiVersion = new ApiVersion(1, 0);
       options.AssumeDefaultVersionWhenUnspecified = true;
       options.ReportApiVersions = true;
       options.ApiVersionReader = ApiVersionReader.Combine(
           new UrlSegmentApiVersionReader(),
           new HeaderApiVersionReader("X-Api-Version")
       );
   }).AddApiExplorer(options =>
   {
       options.GroupNameFormat = "'v'VVV";
       options.SubstituteApiVersionInUrl = true;
   });

   builder.Services.AddControllers();
   builder.Services.AddEndpointsApiExplorer();
   builder.Services.AddSwaggerGen(c =>
   {
       c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API - V1", Version = "v1.0" });
       c.SwaggerDoc("v2", new OpenApiInfo { Title = "My API - V2", Version = "v2.0" });
   });

   var app = builder.Build();

   if (app.Environment.IsDevelopment())
   {
       app.UseSwagger();
       app.UseSwaggerUI();
   }

   app.UseHttpsRedirection();
   app.UseAuthorization();
   app.MapControllers();
   app.Run();
```




