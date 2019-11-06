# Powershell

This is the minimum powershell knowledge you need to have to survive as developer if you work on windows machine.

## Operators

### Logical Operators

| Operator | Description | Example |
| :--- | :--- | :--- |
| `-and` | Logical AND. TRUE when both statements are TRUE | `(1 -eq 1) -and (1 -eq 2)` |
| `-or` | Logical OR. TRUE when either stement is TRUE | `(1 -eq 1) -or (1 -eq 2)` |
| `-xor` | Logical EXCLUSIVE OR. TRUE when only one statement is TRUE | `(1 -eq 1) -xor (2 -eq 2)` |
| `-not` | Logical not. Negates the statement that follows | `-not (1 -eq 1)` |
| `!` | Same as `-not` | `!(1 -eq 1)` |

## Function

### Passing parameters

Pass arguments to a script or cmdlet by separating them with spaces:

```
PS C:\> Get-ChildItem -Path $env:windir -Filter *.dll -Recurse
```

### Param

To define arguments by name, use a param statement, which is simply a comma separated list of variables, optionally prefixed with a \[data type\] and/or with = default values.

    Param(
        [string]$path, 
        [bool]$standalone = $true
    )

    $hostService = split-path $path -leaf `
        | % { $_    -replace("\.", "_") `
                    -replace("-", "_") ` }

### Check if the parameter is null

    Param(
      [string]$targetDirectory
    )

    if (!$targetDirectory) {
      write-host "Please specify your target repo directory.`r`n"
    }

### Call function in another script

Use dot sourcing

```
. .\functions.ps1
```

#### Suppressing return values

It's because PS writes return values to console by default. You want to silence the output often.

```
$appName.Attributes.Append($attrib) | out-null
```

## String

### Substitution

#### Variable substitution

```
$message = "Hello, $first $last."
```

#### Format string

    $appNames `
      | % { 
        '        ' `
        + '<arg transform-app-name="{0}" value="{0}.{1}" xdt:Transform="SetAttributes" xdt:Locator="Match(transform-app-name)" />' `
        -f $_, $location
      }

### select-string for a case-sensitive match in string

This command performs a case-sensitive match of the text that was sent down the pipeline to the Select-String cmdlet.

```
'Hello', 'HELLO' | Select-String -Pattern 'HELLO' -CaseSensitive -SimpleMatch
```

### select-string in text files

This command searches all files with the .txt file name extension in the current directory. The output displays the lines in those files that include the specified string.

```
Get-Alias | Out-File -FilePath .\Alias.txt
Get-Command | Out-File -FilePath .\Command.txt
Select-String -Path .\*.txt -Pattern 'Get'

Alias.txt:8:Alias            cat -> Get-Content
Alias.txt:28:Alias           dir -> Get-ChildItem
Alias.txt:43:Alias           gal -> Get-Alias
Command.txt:966:Cmdlet       Get-Acl
Command.txt:967:Cmdlet       Get-Alias
```

    $foundStrings = get-childItem -recurse -path $path -include app.config, web.config `
        | select-string -pattern "fabric:/" `
        | select-object Line -uniq `

### Search a string in multiple files

    $foundStrings = get-childItem -recurse -path $path -include app.config, web.config `
        | select-string -pattern "fabric:/" `
        | select-object Line -uniq `

### Replace

```
$content -replace "#{database_server_name}#", ".\SQLEXPRESS"
```

## Array

### Removing duplicate values from an array

```
$a = @(1,2,3,4,5,5,6,7,8,9,0,0)
$a = $a | select -Unique
```

    $services = $foundStrings `
        | select-string -pattern "fabric:\/[A-Za-z\.]*" -AllMatches `
        | foreach-object { $_.Matches.Value } `
        | % { $_    -replace("fabric:/", "") `
                    -replace(".ServiceFabric", "") `
                    -replace("\.", "_") `
                    -replace("-", "_") `
            } `
        | ? {$_} `
        | select-object -unique

### Concatenate array into string

```
$OFS = [Environment]::NewLine
"$transformers"
```

## File / Directory

### Directory

#### Get only directories using Get-ChildItem

```
Get-ChildItem -Recurse | ?{ $_.PSIsContainer }
Get-ChildItem -Recurse | ?{ $_.PSIsContainer } | Select-Object FullName
```

#### Create a directory

You can create directories recursively

```
$targetDirectory = $directory + "Build\Build\Hosted\Global\Locations"

if (!(Test-Path "$directory\build")) {
  write-host creating a directory "$directory\build"
  new-item "$directory\build" -ItemType directory
}
```

#### Join Path

```
Join-Path -Path "path" -ChildPath "childpath"
```

### Sends output to a file

```
Get-Process | Out-File -FilePath .\Process.txt
Get-Content -Path .\Process.txt

NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     29    22.39      35.40      10.98   42764   9 Application
     53    99.04     113.96       0.00   32664   0 CcmExec
     27    96.62     112.43     113.00   17720   9 Code
```

    if ($standalone) {
        $outputFile = "digraph G {" + [Environment]::NewLine `
            + "$outputFile}"

        $outputFile | format-table -AutoSize | Out-File "$hostService.md"
    }

### Display file name from the given path

```
Split-Path -Path "C:\Test\Logs\*.log" -Leaf -Resolve
Pass1.log
Pass2.log
...
```

    $hostService = split-path $path -leaf `
        | % { $_    -replace("\.", "_") `
                    -replace("-", "_") ` }

### Reading a text file

```
$content = Get-Content $path
```

### Writing new lines to a text file

    $errorMsg =  "{0} Error {1}{2} key {3} expected: {4}{5} local value is: {6}" -f `
                   (Get-Date),$keyPath,$value,$key,$policyValue,([Environment]::NewLine),$localValue
    Add-Content -Path $logpath $errorMsg

### Console

#### Output to Console

    if (!$targetDirectory) {
      write-host "Please specify your target repo directory.`r`n"
    }

## Object

### Get an object's property by name

    [xml]$config = get-content $path
    $config.configuration.common.logging.factoryAdapter.arg `
      | where-object { $_.key -eq "AppName" } `
      | select -expand "value"

## XML

#### Load into XML object

    [xml]$config = get-content $path
    $config.configuration.common.logging.factoryAdapter.arg `
      | where-object { $_.key -eq "AppName" } `
      | select -expand "value"

#### Add Attribute

      [xml]$config = get-content $path
      $appName = $config.configuration.common.logging.factoryAdapter.arg `
        | where-object { $_.key -eq "AppName" }

      $attrib = $appName.OwnerDocument.CreateAttribute('transform-app-name')
      $attrib.Value = $appname.value
      $appName.Attributes.Append($attrib) | out-null

      write-host "to $path"

      $config.Save($path)



