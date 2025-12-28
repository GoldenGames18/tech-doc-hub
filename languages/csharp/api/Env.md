# 📋Env

Environment variables supply configuration values to the application at startup. They simplify settings via `appsettings.json` during development, but these can be seamlessly adapted for Docker containers or even simple executable launches.

## ⏳Loading Env Manually

```C#
var varible = Environment.GetEnvironmentVariable("my variable"); // string?
```

## { } With appsettings 

### 🏅 What You Need to Know About the Environment Variables Reading Priority Order

1. `appsettings.json`
2. `appsettings.{Environment}.json` (ex : `appsettings.Development.json`)
    1. Env from docker/other
    2. Commande ligne argument
    3. Other

`IConfiguration` is provided by the builder in your ASP.NET Core application.

```csharp
var builder = WebApplication.CreateBuilder(args); builder.Configuration // allows access to your configuration data`
```

### 💡 To facilitate maintainability, I recommend doing this:

1. Create a "Configurations" folder.  
2. In this same folder, create different classes that will be your configuration classes.
3. 
```C#
public class ConsoleConfiguration
{
    public const string ConfigurationName = "Console";
    public string Color { get; set; } = string.Empty;
}
```

To retrieve this data, you will need to use the builder:

```C#

var console = builder.Configuration.GetSection(ConsoleConfiguration.ConfigurationName).Get<ConsoleConfiguration>();
```

## ✅Env Validation
### ⚙️Config

```csharp
builder.Services.AddOptions<>()
  .Bind(builder.Configuration.GetSection(ConsoleConfiguration.ConfigurationName))
  .ValidateDataAnnotations()
  .ValidateOnStart(); 
```

Annotations on configuration objects

```C#
public class ConsoleConfiguration
{
    public const string ConfigurationName = "Console";
    [Required]
    public string Color { get; set; } = string.Empty;
}
```

If `color` is null at startup, it will cause the application to stop.

### 📤Retrieval

1. Injection possible via le constructeur (injection de dépendances).  
2. Or retrieval via `GetRequiredService`.

```C#
GetRequiredService<IOptions<ConsoleConfiguration>>().Value
```


