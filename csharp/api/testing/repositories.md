
## 📦 The Value of Mocks

Why bother spending time creating mocks when we could just test our repositories directly, since in-memory databases exist? Here's the core issue: if your production database is PostgreSQL but you use an in-memory database for testing, this can lead to production problems because some methods aren't compatible across all database types.

## Prerequisites

- **[Testcontainers](https://testcontainers.com)** — Pre-configured modules exist for your database type (search for them 🙃). 
- **Docker** installed on the machine.

## 🐋 Testcontainers 

**Testcontainers** is a library that lets you spin up a Docker container in just a few lines of code, which gets automatically cleaned up and torn down after use.  
It enables you to simulate production environments directly in your tests for maximum reliability.

![Image](https://media1.tenor.com/m/B_zYdea4l-4AAAAC/yay-minions.gif)

## ⚙️ Configurations

1. Define a class that will handle the creation of our Docker container (for this example, I'm using PostgreSQL and the Testcontainers.PostgreSql library)

```C#
public class PostgresContainer : IAsyncDisposable
{
  private readonly PostgreSqlContainer _container = new PostgreSqlBuilder()
    .WithImage("image custom ici, mais ce n’est pas requis pour fonctionner")
    .WithDatabase("my-database")
    .WithUsername("my-username")
    .WithPassword("my-password")
    .Build();

  public string ConnectionString => _container.GetConnectionString();

  public async ValueTask DisposeAsync()
  {
    await _container.StopAsync();
    await _container.DisposeAsync();
    GC.SuppressFinalize(this);
  }

  public async Task StartAsync() => await _container.StartAsync();
}
```

2. Configure a Test DbContext

```C#
public class TestDbContextFactory(string connectionString) : IDbContextFactory<StudentDbContext>
{
  public StudentDbContext CreateDbContext()
  {
    var options = new DbContextOptionsBuilder<StudentDbContext>()
      .UseNpgsql(connectionString)
      .Options;
    return new StudentDbContext(options);
  }
}
```

3. Class that will use both configurations (Example with nunit)
```C#
[SetUpFixture]
public class ConfigSetupContainer
{
  public static PostgresContainer PostgresContainer { get; private set; } = null!;

  [OneTimeSetUp]
  public async Task RunBeforeAnyTests()
  {
    PostgresContainer = new PostgresContainer();
    await PostgresContainer.StartAsync().ConfigureAwait(false);
    await new TestDbContextFactory(PostgresContainer.ConnectionString).CreateDbContext().Database.MigrateAsync();
  }

  [OneTimeTearDown]
  public async Task RunAfterAnyTests() => await PostgresContainer.DisposeAsync().ConfigureAwait(false);
}
```

Now just test the repository and we're good to go