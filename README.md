# About
This is a PowerShell script that leverages the `winget configuration`  command to apply a custom profile to your Windows Installation.
<br>The script will stitch together a winget configuration file based on the profile you chose.
# Usage
One line command: 
 `iwr -useb https://raw.githubusercontent.com/akselkvitberg/winget-dsc/main/apply-configuration.ps1 | iex`

# Resource examples
An assortment of examples for the resources that are available to execute in the configuration.
<br> The full list of resources can be found [here](https://learn.microsoft.com/en-us/powershell/dsc/reference/psdscresources/overview?view=dsc-2.0#resources).

### PowrShell Script
```yaml
- resource: PSDscResources/Script
  id: psscript
  directives:
    description: Create file with PowerShell
  settings:
    GetScript: |
        $fileContent = $null
        $filePath = "C:\Users\user\Desktop\test.txt"  
        if (Test-Path -Path $filePath) {
            $fileContent = Get-Content -Path $filePath -Raw
        }  
        return @{
            Result = $fileContent
        }
    TestScript: |
        $fileContent = $null
        $filePath = "C:\Users\user\Desktop\test.txt"
        if (Test-Path -Path $filePath) {
            $fileContent = Get-Content -Path $filePath -Raw
            return ($fileContent -eq $FileContent)
        } else {
            return $false
        }
    SetScript: |
        $fileContent = "Hello World!"
        $filePath = "C:\Users\user\Desktop\test.txt"
        $streamWriter = New-Object -TypeName 'System.IO.StreamWriter' -ArgumentList @(
            $filePath
        )
        $streamWriter.WriteLine($fileContent)
        $streamWriter.Close()
```
### WinGet package
```yaml
- resource: Microsoft.WinGet.DSC/WinGetPackage
  directives:
    description: Install Visual Studio Code
    allowPrerelease: true
  settings:
    id: Microsoft.VisualStudioCode
    source: winget
    Ensure: Present
```

### Registry Key
```yaml
- resource: PSDscResources/Registry
  id: UpdateStartLayout
  directives:
    description: Updates the start menu layout to less recommendations and more pins
    allowPrerelease: false
  settings:
    Key: HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
    ValueName: Start_Layout
    ValueType: DWord
    ValueData: 1
    Force: true
    Ensure: Present
```