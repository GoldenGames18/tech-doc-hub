# 📗Swashbuckle.AspNetCore.Cli

Swashbuckle.AspNetCore.Cli is a tool that allows generating a swagger json file based on the annotations placed on the controllers
## 📥Install

```bash
 dotnet tool install Swashbuckle.AspNetCore.Cli --local
```

After the installation, the dotnet-tools.json file will have been modified.  
To configure your project to generate a file during compilation, you will need to modify the .csproj file:

```xml
<Target Name="BuildSwaggerFile" AfterTargets="AfterBuild">
	<Exec Command="dotnet tool restore" /> <!-- Install tool -->
	<Exec Command="dotnet swagger tofile"> <!-- generate swagger file -->
</Target>
```

[Example of configuration](https://tonylunt.medium.com/swashbuckle-cli-automating-asp-net-core-api-swagger-definitions-during-build-f3ee2b8e857a)


