# Useful Scripts

## Find all files containing a given string

This will recursively find all files ending is .cs in the directory it is run in down that contain a given string.

```PowerShell
Get-ChildItem -Recurse -File -Filter *.cs | Select-String "some_string" -List
```

The options on both `Get-ChildItem` and `Select-String` can both be modified to change the nature of the search.

See:
- https://stackoverflow.com/questions/8153750/how-to-search-a-string-in-multiple-files-and-return-the-names-of-files-in-powers
- https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem
- https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string


## Convert CSV to Json

Powershell has a cmdlet for converting CSV to Json (there are other conversion cmdlets as well). So if a CSV has been made (manually, code generated, spreadsheet app) and is either a CSV or Tab-Delimited, then the following command will work to parse to Json.

```ps
Import-Csv "YourInput.csv" | ConvertTo-Json | Add-Content -Path "output.json"
```

Further info:
- [`Import-Csv`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/import-csv)
- [`ConvertTo-Json`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/convertto-json)
- [`Add-Content`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/add-content)
- http://brianvanderplaats.com/2015/10/08/generating-json-from-csv-using-powershell/
