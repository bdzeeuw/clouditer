---
layout: page
date: 2020-05-12 15:22:22 -0000
title: Simple tool to find your tenant id
excerpt: "Introduction"
tags: [azure, microsoft azure, azure tenant, azure tenant id, find tenant id, find azure tenant id, tenant id tool]
comments: true
category: blog
---

# Simple script to find the Tenant ID of a Microsoft organization - without requiring access to the tenants Azure Active Directory

## Article

```powershell
<#
    .SYNOPSIS
        Retrieve the tenant ID of the directory.

    .PARAMETER directoryName
        Full Directory Name of Tenant.

    .EXAMPLE
        Get-TenantID  -directoryName 'contoso.onmicrosoft.com'
#>

param
(
    [Parameter(Position = 0, Mandatory = $true)]
    [ValidateNotNullOrEmpty()]
    $directoryName
)

function Get-TenantID
{
    [CmdletBinding()]
    param
    (
        [Parameter(Position = 0, Mandatory = $true)]
        [ValidateNotNullOrEmpty()]
        $directoryName
    )

    $loginDomain = 'login.windows.net'
    $urlPath = '.well-known/openid-configuration'
    $domainSuffix = '.onmicrosoft.com'

    if ( $directoryName.endswith("$domainSuffix") )
        {
            Write-Output "$directoryName ends with $domainSuffix"
        }
    else
        {
            Write-Output "$directoryName does not ends with $domainSuffix, appending $domainSuffix"
            $directoryName += $domainSuffix
        }
    try
        {
            $request = (Invoke-WebRequest https://$loginDomain/$directoryName/$urlPath `
            | ConvertFrom-Json).token_endpoint.Split('/')[3]
            Write-Output "Directory $directoryName has tenant ID: `n$request"
        }
    catch
        {
            Write-Output "Exception Type: $($_.Exception.GetType().FullName)" -ForegroundColor Red
            Write-Output "Exception Message: $($_.Exception.Message)" -ForegroundColor Red
        }
}

Get-TenantID -directoryName $directoryName
```