<h1 align="center">Get List Wifi Passwords for Windows</h1>
<h5 align="center">Backup All Connected Wifi Passwords</h5>

Author: [BinhPo21](https://facebook.com/BinhPo21 "BinhPo21")

<h2>Run with PowerShell (With out Administrator):</h2>

```PowerShell
irm https://tinyurl.com/GetListWifiPasswords | iex
```
<h2> The Wifi password list file will be saved at:</h2>

```Explorer
C:\!WifiPasswords\listWifiPasswords.txt
```
- `[SSID]:[WifiPassowrd]`

<h2>Source Code:</h2>

```PowerShell
# Get List Name Wifi on Windows
$wifiProfiles = netsh wlan show profiles | Select-String -Pattern ":\s*(.*)" | Where-Object { $_.Matches.Groups[1].Value.Trim() -ne "" } | ForEach-Object { $_.Matches.Groups[1].Value.Trim() }
```
```PowerShell
# Patch
$scriptDirectory = "C:\!WifiPasswords"
$fileName = "listWifiPasswords.txt"
```
```PowerShell
# Create a directory if it does not exist
if (-not (Test-Path -Path $scriptDirectory)) {
    New-Item -Path $scriptDirectory -ItemType Directory | Out-Null
}
```
```PowerShell
$passwordList = @()
# Get List Password with List Name Wifi
Write-Host "Getting list Wifi Passwords..."
foreach ($profile in $wifiProfiles) {
    $profileInfo = netsh wlan show profile name="$profile" key=clear
    $password = ($profileInfo[32] -split ": ")[-1].Trim()
    $passwordList += "${profile}:${password}"
}

if ($passwordList) {
    $passwordList | Out-File -FilePath "$scriptDirectory\$fileName"
    Write-Host "Path to list of saved Passwords: $scriptDirectory\$fileName"
    Invoke-Item "$scriptDirectory\$fileName"
}

Read-Host "Press Enter to exit"
exit
```
