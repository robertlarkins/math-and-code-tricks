# Useful Scripts

## Find all files containing a given string

This will recursively find all files in the directory it is run in down that contain a given string.

```PowerShell
Get-ChildItem -Recurse -File -Filter *.cs | Select-String "some_string" -List
```

The options on both `Get-ChildItem` and `Select-String` can both be modified to change the nature of the search.

See:
- https://stackoverflow.com/questions/8153750/how-to-search-a-string-in-multiple-files-and-return-the-names-of-files-in-powers
- https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem
- https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string
