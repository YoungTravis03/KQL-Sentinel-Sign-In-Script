SigninLogs

| where TimeGenerated > ago(30d)
| where UserPrincipalName =~ "user@yourdomain.com"
| extend DeviceName = tostring(DeviceDetail.displayName),
         OS = tostring(DeviceDetail.operatingSystem),
         Browser = tostring(DeviceDetail.browser),
         IsCompliant = tostring(DeviceDetail.isCompliant)

| summarize FirstSeen = min(TimeGenerated), LastSeen = max(TimeGenerated), SignInCount = count() 
    by DeviceName, OS, Browser, IsCompliant, IPAddress, Location
| order by LastSeen desc
