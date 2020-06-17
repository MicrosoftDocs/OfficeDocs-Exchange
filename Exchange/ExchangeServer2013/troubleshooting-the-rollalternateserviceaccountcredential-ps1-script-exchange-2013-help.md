---
title: 'Troubleshoot the RollAlternateServiceAccountCredential.ps1 script'
TOCTitle: Troubleshooting the RollAlternateServiceAccountCredential.ps1 Script
ms:assetid: 2bbf36d3-eb89-4f92-a8de-259a7cb64d62
ms:mtpsurl: https://technet.microsoft.com/library/Ff808310(v=EXCHG.150)
ms:contentKeyID: 63937187
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting the RollAlternateServiceAccountCredential.ps1 Script

_**Applies to:** Exchange Server 2013_

This topic provides solutions and information about common errors that may occur when you use the RollAlternateServiceAccountPassword.ps1 script.

## One or more of the Client Access servers can't be updated with the password

## Problem

When you use the parameters *ToEntireForest* or *ToArrayMembers* with the script, in some instances, one or more of the Client Access servers may not be updated.

## Resolution

Verify the servers the script will target all required servers by using the **Get-ClientAccessArray** cmdlet, as shown in the following example.

```powershell
Get-ClientAccessArray | fl members
```

If the server that's failing to update is a member of the Client Access array and is still not updating correctly, rerun Exchange Setup and add the Client Access server role to the server again. You can also specify individual servers to target using the parameter *ToSpecificServers*.

## Some servers aren't responding to the script

## Problem

In some circumstances, servers might fail to update because of transient errors such as a bad network connection.

## Resolution

Verify that the servers in question have network and Active Directory connectivity, and then try the script again.

## Some array members are out of service for an extended period of time

## Problem

If a server is out of rotation for a longer period of time but is still a member of the array, as determined by the **Get-ClientAccessArray cmdlet**, the script functionality may be impaired when using the parameters *ToArrayMembers* and *ToEntireForest*. The same problem will occur if a server has had a permanent failure but hasn't been cleanly removed from your deployment.

## Resolution

To resolve this issue, remove the server from your deployment using Exchange Setup or run the script in attended mode until the server can be removed.

If the server will only be down for a short time, and you don't want to permanently remove Exchange, you can adjust the script to run against specific servers using the parameter *ToSpecificServers* so that only active servers are targeted. Or, you can remove the RPC Client Access service from the non-responsive server's Active Directory object by using the **Remove-ClientAccessArray** cmdlet, as shown in the following example.

```powershell
Remove-RPCClientAccess -Server Server.Contoso.com
```

After the RPC Client Access service has been removed, the server won't be returned as an array member by [Get-ClientAccessArray](https://docs.microsoft.com/powershell/module/exchange/Get-ClientAccessArray) and the script won't target it. As soon as the server is functional again, you can re-add the RPC Client Access service by using the **New-RpcClientAccess** cmdlet. When the RPC Client Access service is re-added, be sure to restart the Microsoft Exchange Address Book service on the affected server.

> [!WARNING]
> Before you remove the RPC Client Access service from a server, see the topic <A href="https://docs.microsoft.com/powershell/module/exchange/Remove-RpcClientAccess">Remove-RpcClientAccess</A>.

## For More Information

For more information about how to use Kerberos authentication with a Client Access server array or a load-balancing solution, see the following topics:

- [Configuring Kerberos authentication for load-balanced Client Access servers](configuring-kerberos-authentication-for-load-balanced-client-access-servers-exchange-2013-help.md)

- [Using the RollAlternateserviceAccountCredential.ps1 Script in the Shell](using-the-rollalternateserviceaccountcredential-ps1-script-in-the-shell-exchange-2013-help.md)
