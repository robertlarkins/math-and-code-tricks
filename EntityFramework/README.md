# Entity Framework Core

## CLI

Entity Framework has a CLI that allows commands to be run.

If using Visual Studio, the CLI commands can be called through the Package Manager Console.
JetBrains Rider currently does not have an equivalent console.

The CLI commands can instead be called from the terminal by installing the `dotnet-ef` tool.
Generally this will be installed as a global tool:

```ps
dotnet tool install --global dotnet-ef
```

and can then be called using either `dotnet-ef` or `dotnet ef`.

To use `dotnet ef` for a particular project, the project needs to have the Microsoft.EntityFrameworkCore.Design NuGet package.
This can be installed by opening the terminal to the project directory and running the command:
```ps
dotnet add package Microsoft.EntityFrameworkCore.Design
```

See:
- https://docs.microsoft.com/en-us/ef/core/cli/dotnet
