---
title: Using the Exchange Server 2013 Management Pack for troubleshooting
TOCTitle: Using the Exchange Server 2013 Management Pack for troubleshooting
ms:assetid: c9672dad-1e67-4f07-bad9-539a67f2ac70
ms:mtpsurl: https://technet.microsoft.com/library/Dn195913(v=EXCHG.150)
ms:contentKeyID: 53181780
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Using the Exchange Server 2013 Management Pack for troubleshooting

_**Applies to:** Exchange Server 2013_

[Getting started with Exchange Server 2013 Management Pack](getting-started-with-exchange-server-2013-management-pack.md) provides an overview of the management pack dashboard. This topic walks you through how it can help you troubleshoot a problem. The process is best illustrated with an example. Consider the following scenario:

Rob Fielder is an Exchange administrator at Contoso. He opens the SCOM Console and click on the **Server Health** in the Exchange Server 2013 dashboard to check the status of his Exchange servers. He notices a critical state for the **Service Components** on one of his CAS servers.

![Failed CAS server](images/Dn195913.32a265d9-68e0-4d8c-9f83-1d10cdda1f84(EXCHG.150).png "Failed CAS server")

Rob double-clicks on the server which opens the **Health Explorer** window. In this window he can see that the service component that is in an unhealthy state is the OWA.Proxy health set. He click on it to see the relevant information for this health set.

![Failed CAS server healthset details](images/Dn195913.8e4d05a6-9128-40d8-b262-e60e9affc973(EXCHG.150).png "Failed CAS server healthset details")

The link provided under External Knowledge Resources takes Rob to the [Troubleshooting OWA.Proxy Health Set](https://docs.microsoft.com/exchange/management/health/troubleshooting-owa-proxy-health-set) topic. In this article, Rob sees that the first thing to do is to verify that the issue still exists. Following the instructions, he runs the following command to verify the current state of the OWA.Proxy health set in the Shell:

```powershell
Get-ServerHealth Server1.contoso.com | ?{$_.HealthSetName -eq "OWA.Proxy"}
```

Running this command gives him the following output:

```powershell
Server          State           Name                 TargetResource       HealthSetName   AlertValue ServerComponent

------          -----           ----                 --------------       -------------   ---------- ----------
Server1         Online          OWAProxyTestMonitor  MSExchangeOWAAppPool OWA.Proxy       Unhealthy  OwaProxy
Server1         Online          OWAProxyTestMonitor  MSExchangeOWACale... OWA.Proxy       Healthy    OwaProxy
```

Rob sees that the problem lies within the OWA Application Pool. The next step is to rerun the associated probe for the monitor that is in unhealthy state. Using the table in the "Troubleshooting OWA.Proxy Health Set" topic, he determines that the probe that he needs to rerun is OWAProxyTestProbe. He runs the following command:

```powershell
Invoke-MonitoringProbe OWA.Proxy\OWAProxyTestProbe -Server Server1.contoso.com | Format-List
```

He scans the output for the ResultType value and sees that the probe failed:

```powershell
ResultType : Failed
```

He proceeds to the "OWAProxyTestMonitor Recovery Actions" section of the article. He connects to Server1 using IIS Manager to see if the MSExchangeOWAAppPool is running on the IIS Server. Once he verifies that it is running, the next step instructs him to recycle the MSExchangeOWAAppPool:

```powershell
C:\Windows\System32\Inetsrv\Appcmd recycle APPPOOL MSExchangeOWAAppPool
```

After seeing that the MSExchangeOWAAppPool is successfully recycled, he goes back to verifying if the issue still exists by rerunning the probe using the Invoke-MonitoringProbe cmdlet and this time sees that the result is successful. He then runs the following command to verify that the health set is reporting **Healthy** status again:

```powershell
Get-ServerHealth Server1.contoso.com | ?{$_.HealthSetName -eq "OWA.Proxy"}
```

This time he sees that the problem is resolved.

```powershell
Server          State           Name                 TargetResource       HealthSetName   AlertValue ServerComponent

------          -----           ----                 --------------       -------------   ---------- ----------
Server1         Online          OWAProxyTestMonitor  MSExchangeOWAAppPool OWA.Proxy       Healthy    OwaProxy
Server1         Online          OWAProxyTestMonitor  MSExchangeOWACale... OWA.Proxy       Healthy    OwaProxy
```

He goes back to the SCOM console and verifies that the issue is resolved.

![Server Health](images/Dn195908.c863be83-fc4b-4daf-a18b-27b1aae15b1d(EXCHG.150).png "Server Health")

The scenario covered above is a simple demonstration of the troubleshooting workflow when you see an alert in the SCOM console. Even though the details will vary, you will generally follow a similar troubleshooting workflow for each problem reported in the console.
