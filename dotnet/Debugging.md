# Debugging

This describes tricks that can be employed to help debug issues in C# and dotnet.

## Asp.Net startup issue

If when starting up an Asp.Net core application it fails with with an error on the webpage like

> HTTP Error 500.30 - ASP.NET Core app failed to start

Then one approach for helping diagnose the underlying error is to
1. Navigate to where _yourApplicationName.dll_ resides (likely in bin/debug somewhere)
2. Open powershell and run `dotnet yourApplicationName.dll`
3. Check through the generated build messages to see if an error has been generated, and what that error is

See:
- https://stackoverflow.com/questions/67211060/http-error-500-30-asp-net-core-5-app-failed-to-start/67212362#67212362
