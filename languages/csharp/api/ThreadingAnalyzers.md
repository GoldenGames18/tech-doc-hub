# 🧐 Threading analyzers

When developing in C# and writing asynchronous methods, it is recommended to always add the Async keyword to the end of the method name (good practice to follow). To help us follow this convention, there is the Microsoft.VisualStudio.Threading.Analyzers package, which allows adding warnings in the code in case of forgetting this keyword when a method is asynchronous.

## ⚙️Configuration

```xml
<PackageReference Include="Microsoft.VisualStudio.Threading.Analyzers" Version="17.14.15">
  <PrivateAssets>all</PrivateAssets>
  <IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
</PackageReference>
```