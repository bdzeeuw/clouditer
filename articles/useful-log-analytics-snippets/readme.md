---
layout: page
date: 2020-05-12 15:22:22 -0000
title: Useful Log Analytics snippets
excerpt: "Introduction"
tags: [log analytics, loganalytics, azure, microsoft azure, azure monitor, monitoring, kudu, snippets, log analytics snippets]
comments: true
category: blog
---

# Useful Log Analytics snippets

Displaying all 'Allowed' traffic, containing IP address '10.0.1.10'
```sql
AzureDiagnostics
| where Category == "AzureFirewallNetworkRule"
| where msg_s contains "10.0.1.10"
| where msg_s contains "Action: Allow"
| where TimeGenerated > ago(5m)
```

