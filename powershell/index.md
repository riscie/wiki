![](./0.png)
# powershell

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

* [powershell](#powershell)
	* [Everything is an Object](#everything-is-an-object)
	* [Objects have types](#objects-have-types)
	* [Get-Command](#get-command)

<!-- /code_chunk_output -->


## Everything is an Object
* Biggest difference to unix shells, where everything is plain text
* PowerShell is built on top of .NET

## Objects have types
Things which would take more work in bash:
```powershell
PS> 2 + 2
4

PS> 4.2 / 3
1.4

PS> "2" + 2
22  # A string
PS> [Int]"2" + 2
4  # An integer.  The conversion only applies to the "2"
```
## Get-Command / Get-Help
##### Get more information about a command
```powershell
PS> Get-Command Get-Alias | Format-List
```

##### When You're Not Sure What a Command Does
```powershell
PS> Get-Help Get-Alias
PS> Get-Help Get-Alias -examples #example usage only
```

##### When You're Not Sure What Properties Your Object Has
```powershell
PS> Get-Process | Get-Member
```

##### Get all the commands which do something with the noun **Job**
```powershell
PS> Get-Command -noun Job
Cmdlet          Debug-Job                                          3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Get-Job                                            3.0.0.0    Microsoft.PowerShell.Core
...
```

#####Get all commands with the verb **New**
```powershell
PS> Get-Command -verb New
```


## Selecting Properties
```powershell
PS> Get-Process | Select-Object Id, ProcessName
```


## Sorting
```powershell
PS> PS> Get-Process | Sort-Object CPU -descending
```

## Filtering Objects
```powershell
PS> Get-Process | Where-Object WS -gt 150MB
```

```powershell
PS> Get-Process | Sort-Object CPU -last 3
```

### Sources
* [PowerShell Tutorial (Especially for People Who Hate PowerShell)](https://dev.to/rpalo/powershell-tutorial-especially-for-people-who-hate-powershell-2g25)