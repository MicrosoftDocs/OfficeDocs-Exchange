---
localization_priority: Normal
description: 'Summary: Learn how to configure IM integration with Outlook on the web in Exchange 2016 or Exchange 2019.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 0eda267b-41e5-4a60-a209-70a8522a9f41
ms.reviewer: 
title: Configure instant messaging integration with Outlook on the web in Exchange
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure instant messaging integration with Outlook on the web in Exchange

To configure instant messaging (IM) integration between Skype for Business Server and Outlook on the web (formerly known as Outlook Web App) in Exchange 2016 or Exchange 2019, you need to use the Exchange Management Shell. This is different than previous versions of Exchange where you needed to edit the web.config file. If you edit the web.config file instead of using the steps in this topic, the settings are ignored and Outlook on the web users receive the following error message:

 `There's a problem with instant messaging. Please try again later.`

Also, the following health set errors are generated on the Exchange server:

- **HealthSet**: `OWA.Protocol.Dep`

- **Subject**: `OWA.Protocol.Dep health set unhealthy (OwaIMInitializationFailedMonitor/OWA.Protocol.Dep) - Owa InstantMessaging provider failed to intialize`

- **Message**: `Owa InstantMessaging provider failed to initialize due to incorrect IM configuration on the server. Signin attempts to OWA IM will fail. Error Message: {Instant Messaging Certificate Thumbprint is null or empty on web.config).`

Use the procedures in this topic to fix these errors and configure IM integration between Skype for Business Server and Exchange 2016 or Exchange 2019. IM integration between Lync Server 2013 and Exchange 2016 or later isn't supported. For details on setting up Skype for Business Server with Outlook on the web (formerly known as Outlook Web App), see [Configure integration between on-premises Skype for Business Server and Outlook Web App](https://docs.microsoft.com/skypeforbusiness/deploy/integrate-with-exchange-server/outlook-web-app)

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- Exchange and Skype for Business integration requires server certificates that are trusted by all of the servers involved. The procedures in this topic assume that you already have the required certificates. For more information, see [Plan to integrate Skype for Business Server 2015 and Exchange](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/integrate-with-exchange/integrate-with-exchange). The required IM certificate thumbprint refers to the Exchange Server certificate assigned to the IIS service.

- You can only use PowerShell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access virtual directory settings" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- Depending on your Skype for Business Server topology, you may have multiple FrontEnd pools, you should pick the regional endpoint (closest pool to the exchange AD site): `IMServerName=<Skype Server\pool Name>`.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to configure IM integration with Outlook on the web

### Step 1: Specify the IM server and IM certificate thumbprint

Use the following syntax in the Exchange Management Shell to specify the IM server and IM certificate thumbprint:

```powershell
New-SettingOverride -Name "<UniqueOverrideName>" -Component OwaServer -Section IMSettings -Parameters @("IMServerName=<Skype server/pool  name>","IMCertificateThumbprint=<Certificate Thumbprint>") -Reason "<DescriptiveReason>" [-Server <ServerName>]
```

 **Notes:**

- To configure the same settings on all Exchange 2016 and Exchange 2019 servers in the Active Directory forest, don't use the _Server_ parameter.

- To configure the settings on a specific Exchange 2016 or Exchange 2019 server, use the _Server_ parameter and the name of the server (don't use the fully qualified domain name or FQDN). This method is useful when you need to specify different settings on different Exchange servers.

This example specifies the IM server and IM certificate thumbprint on all Exchange 2016 and Exchange 2019 servers in the organization.

- **Setting override name**: "IM Override" (must be unique)

- **Skype for Business server name**: skype01.contoso.com

- **Certificate thumbprint**: CDF34A740E9D225A1A06193A9D44B2CE22775308

- **Override reason**: Configure IM

```powershell
New-SettingOverride -Name "IM Override"  -Component OwaServer -Section IMSettings -Parameters @("IMServerName=skype01.contoso.com","IMCertificateThumbprint=CDF34A740E9D225A1A06193A9D44B2CE22775308") -Reason "Configure IM"
```

This example specifies the IM server and IM certificate thumbprint, but only on the server named Mailbox01.

```powershell
New-SettingOverride -Name "Mailbox01 IM Override"  -Component OwaServer -Section IMSettings -Parameters @("IMServerName=skype01.contoso.com","IMCertificateThumbprint=CDF34A740E9D225A1A06193A9D44B2CE22775308") -Reason "Configure IM" -Server Mailbox01
```

### Step 2: Refresh the IM settings on the Exchange server

Use the following syntax in the Exchange Management Shell to refresh the IM settings on the server. You need to do this on every Exchange 2016 or Exchange 2019 server that's used for Outlook on the web.

```powershell
Get-ExchangeDiagnosticInfo -Server <ServerName> -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh
```

This example refreshes the IM settings on the server named Mailbox01.

```powershell
Get-ExchangeDiagnosticInfo -Server Mailbox01 -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh
```

### Step 3: Restart the Outlook on the web pool on the Exchange server

Run the following command in the Exchange Management Shell or in Windows PowerShell on the server. You need to do this on every Exchange 2016 or Exchange 2019 server that's used for Outlook on the web.

```powershell
Restart-WebAppPool MSExchangeOWAAppPool
```

## How do you know this worked?

You'll know that you've successfully configured IM integration with Outlook on the web when the error message goes away, and clients are able to sign in to IM.

To verify the values of the **IMServerName** and **IMCertificateThumbprint** properties on an Exchange server, replace _\<ServerName\>_ with the name of the server (not the FQDN), and run the following command:

```powershell
[xml]$diag=Get-ExchangeDiagnosticInfo -Server <ServerName> -Process MSExchangeMailboxAssistants -Component VariantConfiguration -Argument "Config,Component=OwaServer"; $diag.Diagnostics.Components.VariantConfiguration.Configuration.OwaServer.IMSettings
```

 **Note**: In Exchange 2016 CU3 or earlier, you need to use different values for some of the parameters:

- _Process_: `Microsoft.Exchange.Directory.TopologyService` (instead of `MSExchangeMailboxAssistants`).

- _Argument_: `Config` (instead of `"Config,Component=OwaServer"`).
