
## 📜 Prerequisites

- Use the test database configuration   
- [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)  
- Modify the Program.cs file and add ⇒ public abstract partial class Program;

## ⚙️ Configurations

Allows removing the base DbContext implementation to use the test one

```C#
public class WebApplicationTest : WebApplicationFactory<Program>
{
  protected override void ConfigureWebHost(IWebHostBuilder builder)
  {
    builder.ConfigureTestServices(services =>
    {
      services.RemoveAll<DbContextOptions<StudentDbContext>>();
      services.AddDbContextFactory<StudentDbContext>(options => options.UseNpgsql("connection string"));
    });
  }
}
```

Or load a configuration from an appsettings file in the test project:

```C#
public class WebApplicationTest : WebApplicationFactory<Program>
{
  protected override void ConfigureWebHost(IWebHostBuilder builder)
  {
	  builder.ConfigureAppConfiguration((context, config) =>
    {
      config.AddJsonFile("appsettings.Test.json", optional: true, reloadOnChange: false);
      config.AddInMemoryCollection(new Dictionary<string, string?>
      {
        ["Database:ConnectionStrings:DatabaseConnection"] = "my connection string" 
      });
    });
  }
}
```

Add this line to the project (allows keeping the build version and therefore for a modification it will be necessary to stop the tests and restart them)

```C#
  <ItemGroup>
    <Content Include="appsettings.Test.json">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
  </ItemGroup>
```

## ⚖️Inheritance

It allows you to avoid code duplication across your numerous tests and facilitates maintainability.  
The `JsonSerializerOptions` enables converting enums to strings instead of just returning integers.

```C#
public class ControllerTestBase
{
  protected WebApplicationTest WebApplicationTest { get; private set; }

  protected JsonSerializerOptions Options { get; private set; }

  [OneTimeSetUp]
  public async Task OneTimeSetUp()
  {
    Options = new JsonSerializerOptions(JsonSerializerDefaults.Web);
    Options.Converters.Add(new JsonStringEnumConverter());
    WebApplicationTest = new WebApplicationTest();
    var context = await WebApplicationTest.Services.GetRequiredService<IDbContextFactory<StudentDbContext>>().CreateDbContextAsync();
    await context.Database.MigrateAsync();
  }

  [OneTimeTearDown]
  public async Task OneTimeTearDown() => await WebApplicationTest.DisposeAsync();
}
```

## 💡 Example

### ⚠️ Get 400

```C#
  [Test]
  public async Task GetStudentBadRequest()
  {
    using var httpClient = WebApplicationTest.CreateClient();
    var response = await httpClient.GetAsync($"/students");
    Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.BadRequest));
  }
```

### ✅ Get 200 with data validation

```C#
[Test]
  public async Task GetStudentBadRequest()
  {
    var context = await WebApplicationTest.Services.GetRequiredService<IDbContextFactory<StudentDbContext>>().CreateDbContextAsync();
    using var httpClient = WebApplicationTest.CreateClient();
    var response = await httpClient.GetFromJsonAsync<StudentDto>($"/students", Options);
    //Assert here
  }
```