---
layout: blog
date: 2020-05-25 15:22:22 -0000
title: PowerShell Password State API example
excerpt: "Introduction"
tags: [powershell, passwordstate, passwordstate api, passwordstate api powershell, passwordstate]
comments: true
category: blog
---

# PowerShell Password State API example

## Article
The function:
```powershell

<#
    Retrieves password record from Passwordstate
    Example: $creds = Get-Passwordstate -id <ID> -apikey <APIKEY>
#>

function Get-Passwordstate {

    [CmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True,
        Position=0,
        HelpMessage="Enter the password ID")]
        [string]
        $id,

        [Parameter(Mandatory=$True,
        Position=1,
        HelpMessage="Enter the API key")]
        [string]
        $apikey
    )

	$url = 'https://passwordstate.contoso.com/api/passwords'

    Invoke-RestMethod -Uri $url/$($id)?apikey=$($apikey) -Method Get

}
```

Example of retreiving a password using above's function
```powershell
<#
    Retrieves password record from Passwordstate
    Example: $creds = Get-Passwordstate -id <ID> -apikey <APIKEY>
#>

param(
    [Parameter(Mandatory=$True,
    Position=0,
    HelpMessage="Enter the password ID")]
    [string]
    $id,

    [Parameter(Mandatory=$True,
    Position=1,
    HelpMessage="Enter the API key")]
    [string]
    $apikey
)

#Import Get-Password function
. .\get-password.ps1

$PasswordstateCreds = Get-Passwordstate -id $id -apikey $apikey
$User = $PasswordstateCreds.Username
$Password = ConvertTo-SecureString -String $PasswordstateCreds.Password -AsPlainText -Force
$Credentials = New-Object System.Management.Automation.PSCredential ($User, $Password)
```