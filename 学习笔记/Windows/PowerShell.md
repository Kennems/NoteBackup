---
title : 'PowerShell'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "function Get-FrenchHoliday"
image : img/cat.jpg
draft : false
categories : ["Windows"]
tags : ["学习笔记", "Windows"]
---
# PowerShell

```powershell
function Get-FrenchHoliday
{
    param
    (
        [int]
        $Year = (Get-Date).Year,

        [ValidateSet("alsace-moselle", "guadeloupe", "guyane", "la-reunion", "martinique", "mayotte", "metropole", "nouvelle-caledonie", "polynesie-francaise", "saint-barthelemy", "saint-martin", "saint-pierre-et-miquelon", "wallis-et-futuna")]
        [string]
        $Area = 'metropole',

        [switch]
        $NextOnly
    )

    $url = "https://calendrier.api.gouv.fr/jours-feries/$Area/$Year.json"

    $holidays = Invoke-RestMethod -Uri $url -UseBasicParsing

    foreach ($obj in $holidays.PSObject.Properties) {
        if (-Not ($NextOnly.IsPresent) -or (((([DateTime]$obj.Name).Ticks) - (Get-Date).Ticks) -gt 0)) {
            Write-Host "$($obj.Value) : $($obj.Name)"
        }
    }
}
```

```powershell
Get-FrenchHoliday
```

