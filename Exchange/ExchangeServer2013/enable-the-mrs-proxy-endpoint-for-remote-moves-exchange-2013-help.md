---
title: 'Enable the MRS Proxy endpoint for remote moves: Exchange 2013 Help'
TOCTitle: Enable the MRS Proxy endpoint for remote moves
ms:assetid: 9840f712-127e-4c2d-bfe5-1b35cdb2a31b
ms:mtpsurl: https://technet.microsoft.com/library/Dn155787(v=EXCHG.150)
ms:contentKeyID: 53903965
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable the MRS Proxy endpoint for remote moves

_**Applies to:** Exchange Server 2013_

The Mailbox Replication Service Proxy (MRS Proxy) facilitates cross-forest mailbox moves and remote move migrations between your on-premises Exchange organization and Exchange Online. In Exchange 2013, MRS Proxy is included in the Mailbox server role (also called the *Mailbox server*). During cross-forest and remote move migrations, a Client Access server acts as a proxy for incoming move requests for the Mailbox server. The ability of a Client Access server to accept these requests is disabled by default. To allow the Client Access server to accept incoming move requests, you have to enable the MRS Proxy endpoint.

The Client Access server on which to enable the MRS Proxy endpoint depends on the type and direction of the mailbox move.

- **Cross-forest enterprise moves**: For cross-forest moves that are initiated from the target environment (known as a *pull* move type), you have to enable the MRS Proxy endpoint on Client Access servers in the source environment. For cross-forest moves that are initiated from the source environment (known as a *push* move type), you have to enable the MRS Proxy endpoint on Client Access servers in the target environment.

- **Remote move migrations between an on-premises Exchange organization and Exchange Online**: For both onboarding and offboarding remote move migrations, you have to enable the MRS Proxy endpoint on Client Access servers in your on-premises organization.

> [!NOTE]
> If you use the EAC to move mailboxes, cross-forest moves and onboarding remote move migrations are pull move types because the request is initiated from the target environment. Offboarding remote move migrations are a push move type because the request is initiated from the source environment.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes per server.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange Web Services permissions" section in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- If you've deployed more than one Client Access server in your Exchange organization, you should enable the MRS Proxy endpoint on each one. If you add additional Client Access servers, be sure to enable the MRS Proxy endpoint on the new servers. Cross-forest moves and remote move migrations can fail if the MRS Proxy endpoint isn't enabled on all Client Access servers.

- If you don't perform cross-forest moves or remote move migrations, keep MRS Proxy endpoints disabled to reduce the attack surface of your organization.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to enable the MRS Proxy endpoint

1. In the EAC, navigate to **Recipients** \> **Servers** \> **Virtual Directories**.

2. In the **Select server** drop-down list, select the name of the Client Access server on which you want to enable the MRS Proxy endpoint. Or select **All servers** to display the virtual directories on all Client Access servers in your organization.

3. In the **Select type** drop-down list, select **EWS** to display the Exchange Web Service (EWS) virtual directory for the selected server.

4. In the list of virtual directories, click **EWS (Default Web Site)** for the Client Access server that you want to configure, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

5. On the **EWS (Default Web Site)** properties page, select the **Enable MRS Proxy endpoint** check box, and then click **Save**.

## Use the Shell to enable the MRS Proxy endpoint

The following command enables the MRS Proxy endpoint on a Client Access server named EXCH-SRV-01.

```powershell
Set-WebServicesVirtualDirectory -Identity "EXCH-SRV-01\EWS (Default Web Site)" -MRSProxyEnabled $true
```

The following command enables the MRS Proxy endpoint on all Client Access servers in your Exchange organization.

```powershell
Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -MRSProxyEnabled $true
```

> [!IMPORTANT]
> As previously stated, the MRS Proxy endpoint should be enabled on each Client Access server in your organization. Run the preceding command after adding a new Client Access server to your organization.

## How do you know this worked?

To verify that you've successfully enabled the MRS Proxy endpoint, do one of the following:

1. In the EAC, navigate to **Recipients** \> **Servers** \> **Virtual Directories**.

2. In the list of virtual directories, click **EWS (Default Web Site)** and verify in the details pane that the MRS Proxy endpoint is enabled.

    Alternatively, you can click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon") to view the **EWS (Default Web Site)** properties page and verify that the **Enable MRS Proxy endpoint** check box is selected.

Or

Run the following command in the Shell:

```powershell
Get-WebServicesVirtualDirectory | FL Identity,MRSProxyEnabled
```

Verify that the *MRSProxyEnabled* parameter is set to `True`.

Another way to verify that the MRS Proxy endpoint is enabled is to use the **Test-MigrationServerAvailability** cmdlet to test the ability to communicate with the remote server that hosts the mailboxes that you want to move, or in the case of offboarding Exchange Online mailboxes to your on-premises organization, a server in your on-premises organization. For more information, see [Test-MigrationServerAvailability](https://docs.microsoft.com/powershell/module/exchange/Test-MigrationServerAvailability).

The following example tests the connection to a server in the corp.contoso.com forest.

```powershell
$Credentials = Get-Credential
```

```powershell
Test-MigrationServerAvailability -ExchangeRemoteMove -Autodiscover -EmailAddress administrator@corp.contoso.com -Credentials $Credentials
```

To run this command successfully, the MRS Proxy endpoint must be enabled.
