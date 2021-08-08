# PowerShell

## Functions

Function signatures should not be written in the form `MyFunction($param1, $param2)` as when called using `$result = MyFunction(1, 2)`
PowerShell sees `(1, 2)` not as two parameters but as an array with two elements.

See:
 - https://serverfault.com/questions/819763/why-is-it-wrong-to-call-functions-with-parentheses-in-powershell

## Variables

Variable names are not case sensitive

 - https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_variables?view=powershell-7.1
