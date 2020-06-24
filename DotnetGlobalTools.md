# DotNet Global Tools


## Installation


## Updating


## 401 (Unauthorized) Error
When installing a dotnet tool this error may occur. Unsure of the exact cause, but adding the flag
```
--ignore-failed-sources
```
to the install command should resolve the issue.

## Tools

### Report Generator
https://www.nuget.org/packages/dotnet-reportgenerator-globaltool/

```ps
dotnet tool install --global dotnet-reportgenerator-globaltool
```
