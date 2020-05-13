---
layout: page
date: 2020-05-12 15:22:22 -0000
title: Simple script to clone all repositories within a GitLab Group
excerpt: "Introduction"
tags: [powershell, gitlab, gitlab clone, gitlab clone powershell, gitlab api powershell]
comments: true
category: blog
---

# Simple script to clone all repositories within a GitLab Group

## Article

```powershell
param(
    [Parameter(Mandatory=$True,HelpMessage = 'Enter Personal Access Token')]
    [Security.SecureString] $token
)

$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($token)
$plainToken = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
$gitlab_host = "https://git.contoso.com"
$required_group = "group"
# Retrieve list of all repositories
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("PRIVATE-TOKEN", $plainToken)
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$groups = Invoke-RestMethod $gitlab_host'/api/v4/groups?per_page=100' -Headers $headers

foreach($group in $groups)
{
    if ($group.name -eq $required_group)
    {
        Write-Host $group.name"matched"$group.id
        $groupid = $group.id
        $projects = Invoke-RestMethod $gitlab_host'/api/v4/groups/'$groupid'/projects?per_page=100' -Headers $headers
		foreach ($project in $projects)
		{
            $project.http_url_to_repo
            $url = ($project.http_url_to_repo).replace("https://","https://gitlab-ci-token:$plainToken@")
            git clone $url
        }
    }
}
```