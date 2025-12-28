
# 🌐 Web Api in c\#

Creating an API is done using [ASP.NET](http://asp.net/) Core Web API.  
A project template is available in Visual Studio, and it’s recommended to use it with controllers.  

Example of a default controller:
```csharp
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase {

	[HttpPost]
	[ProducesResponseType(StatusCodes.Status201Created)]
	[ProducesResponseType(StatusCodes.Status400BadRequest)]
	public ActionResult<Pet> Create(Pet pet)
	{
	    pet.Id = _petsInMemoryStore.Any() ? 
	             _petsInMemoryStore.Max(p => p.Id) + 1 : 1;
	    _petsInMemoryStore.Add(pet);
	
	    return CreatedAtAction(nameof(GetById), new { id = pet.Id }, pet);
	}
}
```

## 📝 Notes :

 - [🛡️ Security](Security.md)
- [🛢EF core](EFcore.md)
- [🧪 Testing](testing/index.md)
- [✋🏻 Cancel Request](CancelRequest.md)
- [🧐 Threading analyzers](ThreadingAnalyzers.md)
- [🪪 Identity framwork ](Identity.md)
- [🗺️ Mapper](mapper/index.md)
- [🧵 Serilog](Serilog.md)
- [✨Tips](Tips.md)
- [🚧 Channel](Channel.md)
- [🔒 Mutex](Mutex.md)
- [🚃 Semaphore](Semaphore.md)
- [🌄 Background service](BackgroundService.md)
- [📋Env](Env.md)
- [🏷️ Version control](VersionControl.md)
- [🔧Tools](tools/index.md)
- [🧮gRPC](advanced/GRPC.md)




