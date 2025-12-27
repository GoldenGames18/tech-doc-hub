# 🧬Unit Test

There are three libraries to test your code :
- [MsTest](https://learn.microsoft.com/fr-fr/dotnet/core/testing/unit-testing-csharp-with-mstest)
- [NUnit](https://nunit.org/)
- [XUnit](https://xunit.net/?tabs=cs)

## 🔬 Comparison: xUnit vs MSTest vs NUnit

| Feature | **xUnit** 🎯 | **MSTest** ✅ *Native Microsoft* | **NUnit** 🔧 |
|---------|--------------|--------------------------------|-------------|
| **Maintainer** | Community (ex-Microsoft) | **Microsoft** (official .NET) | Community |
| **Parallel Execution** | ✅ Default | ⚠️ Configurable | ⚠️ `[Parallelizable]` |
| **Test Attribute** | `[Fact]` / `[Theory]` | `[TestMethod]` | `[Test]` |
| **Setup/Teardown** | Constructor / `IDisposable` | `[TestInitialize]` / `[TestCleanup]` | `[SetUp]` / `[TearDown]` |
| **Speed** | 🚀 **Fastest** | 🟡 Medium | 🟡 Medium |
| **Assertions** | Minimal (use FluentAssertions) | Built-in | **Rich set** |
| **VS Integration** | Good | **Best** (native) | Good |
| **Best For** | Modern .NET / TDD | **Microsoft stack** | Complex scenarios |

---

These libraries are the **core of unit testing**, and we will use other libraries to enhance our testing experience.

### 📦 Mock 

For all tests that require **mocking**, we used to rely on **Moq**, a popular .NET library. However, it introduced an update that added an **email address sniffer**, causing many developers to remove this library from their projects. Even though the email sniffer was later removed, the **trust between developers and this library was broken**.

Resource : https://www.youtube.com/watch?v=A06nNjBKV7I&t=100s

Its current replacement is none other than [NSubstitute](https://nsubstitute.github.io/)

```csharp
//Create:
var calculator = Substitute.For<ICalculator>();

//Set a return value:
calculator.Add(1, 2).Returns(3);
Assert.AreEqual(3, calculator.Add(1, 2));

//Check received calls:
calculator.Received().Add(1, Arg.Any<int>());
calculator.DidNotReceive().Add(2, 2);

//Raise events
calculator.PoweringUp += Raise.Event();

```

## 📢Assert

Each testing library adds its own **Assert implementation**, which is not very practical. That's why many people decided to create a new assertion library based on **extension methods**. This one is called **[AwesomeAssertions](https://awesomeassertions.org/)** and is a fork of a library that became paid.

``` csharp
string accountNumber = "1234567890";
accountNumber.Should().Be("0987654321");

```