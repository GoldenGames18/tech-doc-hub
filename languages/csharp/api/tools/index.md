# 🔧 Tools dotnet

The Tool manifest file is a JSON file named dotnet-tools.json which is created in the project's .config folder (often at the root of the repository). It serves to list and manage the local .NET tools (CLI) used for this project. AND this one will reference all the tools used in your application

## 🎨Creation

```bash
dotnet new tool-manifest  
```

Puts the JSON file in the project's .config folder.

```json
﻿{
  "version": 1,
  "isRoot": true,
  "tools": {,
  }
}
```

## ➕Adding a tool

```bash
 dotnet tool install YOUR_TOOL_NAME_HERE --local
```


## 📝Tools List

- [📗Swashbuckle.AspNetCore.Cli](SwaggerCli.md)