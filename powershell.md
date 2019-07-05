# Powershell

This is the minimum powershell knowledge you need to have to survive as developer if you work on windows machine.

* [Parameters](powershell.md#parameters)
* [Finds text in strings and files](powershell.md#finds-text-in-strings-and-files)
* [Removing duplicate values from an array](powershell.md#removing-duplicate-values-from-an-array)
* [Sends output to a file](powershell.md#sends-output-to-a-file)
* [Display file name from the given path](powershell.md#display-file-name-from-the-given-path)
* [Writing new lines to a text file](powershell.md#writing-new-lines-to-a-text-file)
* [Get only directories using Get-ChildItem](powershell.md#get-only-directories-using-get-childitem)

## Parameters

### Passing parameters

Pass arguments to a script or cmdlet by separating them with spaces:

```text
PS C:\> Get-ChildItem -Path $env:windir -Filter *.dll -Recurse
```

#### Param

To define arguments by name, use a param statement, which is simply a comma separated list of variables, optionally prefixed with a \[data type\] and/or with = default values.

```text
Param(
    [string]$path, 
    [bool]$standalone = $true
)

$hostService = split-path $path -leaf `
    | % { $_    -replace("\.", "_") `
                -replace("-", "_") ` }
```

#### Check if the parameter is null

```bash
Param(
  [string]$targetDirectory
)

if (!$targetDirectory) {
  write-host "Please specify your target repo directory.`r`n"
}
```

### Finds text in strings and files

#### Find a case-sensitive match

This command performs a case-sensitive match of the text that was sent down the pipeline to the Select-String cmdlet.

```text
'Hello', 'HELLO' | Select-String -Pattern 'HELLO' -CaseSensitive -SimpleMatch
```

#### Find matches in text files

This command searches all files with the .txt file name extension in the current directory. The output displays the lines in those files that include the specified string.

```text
Get-Alias | Out-File -FilePath .\Alias.txt
Get-Command | Out-File -FilePath .\Command.txt
Select-String -Path .\*.txt -Pattern 'Get'

Alias.txt:8:Alias            cat -> Get-Content
Alias.txt:28:Alias           dir -> Get-ChildItem
Alias.txt:43:Alias           gal -> Get-Alias
Command.txt:966:Cmdlet       Get-Acl
Command.txt:967:Cmdlet       Get-Alias
```

```text
$foundStrings = get-childItem -recurse -path $path -include app.config, web.config `
    | select-string -pattern "fabric:/" `
    | select-object Line -uniq `
```

#### Search a string in multiple files

```text
Get-ChildItem $path `
    | ? { $_.PSIsContainer } `
    | resolve-path `
    | % { 
        $output = .\find-sfservices-graphviz $_ $false
        write-host $output
        add-content .\"$directoryName.md" $output
    }
```

### Removing duplicate values from an array

```text
$a = @(1,2,3,4,5,5,6,7,8,9,0,0)
$a = $a | select -Unique
```

```text
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

```

### Sends output to a file

```text
Get-Process | Out-File -FilePath .\Process.txt
Get-Content -Path .\Process.txt

NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     29    22.39      35.40      10.98   42764   9 Application
     53    99.04     113.96       0.00   32664   0 CcmExec
     27    96.62     112.43     113.00   17720   9 Code
```

```text
if ($standalone) {
    $outputFile = "digraph G {" + [Environment]::NewLine `
        + "$outputFile}"

    $outputFile | format-table -AutoSize | Out-File "$hostService.md"
}
```

### Display file name from the given path

```text
Split-Path -Path "C:\Test\Logs\*.log" -Leaf -Resolve
Pass1.log
Pass2.log
...
```

```text
$hostService = split-path $path -leaf `
    | % { $_    -replace("\.", "_") `
                -replace("-", "_") ` }

```

### Writing new lines to a text file

```text
$errorMsg =  "{0} Error {1}{2} key {3} expected: {4}{5} local value is: {6}" -f `
               (Get-Date),$keyPath,$value,$key,$policyValue,([Environment]::NewLine),$localValue
Add-Content -Path $logpath $errorMsg
```

### Get only directories using Get-ChildItem

```text
Get-ChildItem -Recurse | ?{ $_.PSIsContainer }
Get-ChildItem -Recurse | ?{ $_.PSIsContainer } | Select-Object FullName
```

#### Output to Console

```bash
if (!$targetDirectory) {
  write-host "Please specify your target repo directory.`r`n"
}
```



