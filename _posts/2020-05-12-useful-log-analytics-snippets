---
layout: post
title: Useful Log Analytics snippets
excerpt: "Introduction"
tags: [log analytics, loganalytics, azure, microsoft azure, azure monitor, monitoring, kudu, snippets, log analytics snippets]
comments: true
category: blog
---

Displaying all 'Allowed' traffic, containing IP address '10.0.1.10'
```sql
AzureDiagnostics
| where Category == "AzureFirewallNetworkRule"
| where msg_s contains "10.0.1.10"
| where msg_s contains "Action: Allow"
| where TimeGenerated > ago(5m)
```

