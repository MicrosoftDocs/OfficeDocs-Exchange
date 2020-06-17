---
localization_priority: Normal
description: 'Summary: Learn how to enable or disable MAPI over HTTP in your Exchange 2016 or Exchange 2019 organization.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 2c07b1e6-8d07-4e73-8800-b306e2266c7d
ms.reviewer:
title: Configure MAPI over HTTP in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure MAPI over HTTP in Exchange Server

In Exchange 2016 and Exchange 2019, you can configure MAPI over HTTP at the organization level or at the individual mailbox level. Mailbox-level settings always take precedence over organization-wide settings.

The scenarios where MAPI over HTTP is enabled or disabled by default at the organization level are described in the following table:

||**Exchange 2019**|**Exchange 2016**|
|:-----|:-----|:-----|
|**Upgrading from an Exchange 2016 environment**|MAPI over HTTP is enabled by default|n/a|
|**Upgrading from an environment that contains any Exchange 2013 servers**|MAPI over HTTP is disabled by default|MAPI over HTTP is disabled by default|
|**Upgrading from an Exchange 2010 environment**|n/a|MAPI over HTTP is enabled by default|

> [!NOTE]
> When MAPI over HTTP is enabled at the organization level, the _MapiHttpEnabled_ property value that's returned by the **Get-OrganizationConfig** cmdlet is `True`.

This topic describes how to configure and then enable MAPI over HTTP for Exchange organizations that contain Exchange 2013 servers, or for any topology where MAPI over HTTP has been previously disabled. You can also use the procedures in this article to disable MAPI over HTTP at the organization level.

 This topic also describes how to enable or disable MAPI over HTTP for an individual mailbox. At the mailbox level, you have the ability to allow or block MAPI over HTTP connections internally, externally, or both. In all cases, when MAPI over HTTP is disabled, connections will be made with Outlook Anywhere.

## Configure MAPI over HTTP

Complete the following steps to configure MAPI over HTTP for your organization. These steps assume you have already configured the prerequisites described in [MAPI over HTTP in Exchange Server](mapi-over-http.md). Once configured (steps 1-3), use step 4 to enable or disable specific permission scenarios at the organization level, at the mailbox level, or both.

1. **Virtual directory configuration**: By default, Exchange creates a virtual directory for MAPI over HTTP. You use the **Set-MapiVirtualDirectory** cmdlet to configure the virtual directory. You need to configure an internal URL, an external URL, or both. For more information see, [Set-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/set-mapivirtualdirectory).

    For example, to configure the default MAPI virtual directory on the local Exchange server by setting the internal URL value to https://contoso.com/mapi, and the authentication method to `Negotiate`, run the following command:

    ```powershell
    Set-MapiVirtualDirectory -Identity "Contoso\mapi (Default Web Site)" -InternalUrl   https://Contoso.com/mapi -IISAuthenticationMethods Negotiate
    ```

2. **Certificate configuration**: The digital certificate used by your Exchange environment must include the same _InternalURL_ and _ExternalURL_ values that are defined on the MAPI virtual directory. For more information on Exchange certificate management, see [Digital certificates and encryption in Exchange Server](../../architecture/client-access/certificates.md). Make sure the Exchange certificate is trusted on the Outlook client workstation and that there are no certificate errors, especially when you access the URLs configured on the MAPI virtual directory.

3. **Update server rules**: Verify that your load balancers, reverse proxies, and firewalls are configured to allow access to the MAPI over HTTP virtual directory.

4. Use the following steps to enable MAPI over HTTP in your entire Exchange organization, or enable MAPI over HTTP for one or more individual mailboxes.

    > [!NOTE]
    > After you run the commands below, Outlook clients with MAPI over HTTP enabled will see a message to restart Outlook to use MAPI over HTTP.

    **Enable MAPI over HTTP in your Exchange organization**

    To enable or disable MAPI over HTTP at the organizational level, use the **Set-OrganizationConfig** cmdlet with the _MapiHttpEnabled_ parameter. Valid values are:

    - **$true**: MAPI over HTTP connections are allowed for all mailboxes in the organization (unless MAPI over HTTP is disabled on a specific mailbox).

    - **$false**: MAPI over HTTP connections aren't allowed for all mailboxes in the organization (unless MAPI over HTTP is enabled on a specific mailbox).

    The following example enables MAPI over HTTP connections for the entire organization:

    ```powershell
    Set-OrganizationConfig -MapiHttpEnabled $true
    ```

    **Enable MAPI over HTTP for an individual mailbox**

    To enable or disable MAPI over HTTP at the mailbox level, use the **Set-CasMailbox** cmdlet with the _MapiHttpEnabled_ parameter. Valid values are:

    - **$null**: The mailbox follows organization-level settings. This is the default value.

    - **$true**: Enable MAPI over HTTP for the mailbox. If MAPI over HTTP is disabled at the organizational level, it's enabled for the mailbox.

    - **$false**: Disable MAPI over HTTP for the mailbox. If MAPI over HTTP is enabled at the organizational level, it's disabled for the mailbox, so the mailbox will use Outlook Anywhere connections.

    The following example enables MAPI over HTTP connections for a single mailbox:

    ```powershell
    Set-CasMailbox <user or mailbox ID> -MapiHttpEnabled $true
    ```

### Test MAPI over HTTP connections

You can test the end-to-end MAPI over HTTP connection by using the **Test-OutlookConnectivity** cmdlet. To use the **Test-OutlookConnectivity** cmdlet, the Microsoft Exchange Health Manager (MSExchangeHM) service must be started.

The following example tests the MAPI over HTTP connection from the Exchange server named ContosoMail.

```powershell
Test-OutlookConnectivity -RunFromServerId ContosoMail -ProbeIdentity OutlookMapiHttpSelfTestProbe
```

A successful test returns output that's similar to the following example:

```
MonitorIdentity                                          StartTime              EndTime                Result      Error     Exception
---------------                                          ---------              -------                ------      -----     ---------
OutlookMapiHttp.Protocol\OutlookMapiHttpSelfTestProbe    2/14/2018 7:15:00 AM   2/14/2018 7:15:10 AM   Succeeded
```

For more information, see [Test-OutlookConnectivity](https://docs.microsoft.com/powershell/module/exchange/test-outlookconnectivity).

Logs for MAPI over HTTP activity are at the following locations:

- %ExchangeInstallPath%Logging\MAPI Address Book Service\

- %ExchangeInstallPath%Logging\MAPI Client Access\

- %ExchangeInstallPath%Logging\HttpProxy\Mapi\

## Combining MAPI over HTTP configurations and internal or external connections

In addition to the organization and mailbox settings described earlier in this topic, you can use the _MapiBlockOutlookExternalConnectivity_ parameter on the  **Set-CasMailbox** cmdlet to allow or deny external Outlook Anywhere or MAPI over HTTP connections to a specific mailbox. Valid values are:

- **True**: Only internal connections are allowed to the mailbox.

- **False**: Internal and external connections are allowed to the mailbox. This is the default value.

The following table summarizes the results of the different setting combinations at the organization level and on individual mailboxes.

|**MapiHttpEnabled value on Set-OrganizationConfig**|**MapiHttpEnabled value on Set-CasMailbox**|**MapiBlockOutlookExternalConnectivity value on Set-CasMailbox**|**AutoDiscover result**|
|:-----|:-----|:-----|:-----|
|$true|$null|$false|MAPI over HTTP, internal and external|
|$true|$null|$true|MAPI over HTTP, internal only|
|$true|$true|$false|MAPI over HTTP, internal and external|
|$true|$true|$true|MAPI over HTTP, internal only|
|$true|$false|$false|Outlook Anywhere, internal and external|
|$true|$false|$true|Outlook Anywhere, internal only|
|$false|$null|$false|Outlook Anywhere, internal and external|
|$false|$null|$true|Outlook Anywhere, internal only|
|$false|$true|$false|MAPI over HTTP, internal and external|
|$false|$true|$true|MAPI over HTTP, internal only|
|$false|$false|$false|Outlook Anywhere, internal and external|
|$false|$false|$true|Outlook Anywhere, internal only|

## Manage MAPI over HTTP

You can manage the configuration of MAPI over HTTP by using the following cmdlets:

- [Set-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/set-mapivirtualdirectory)

- [Get-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/get-mapivirtualdirectory)

- [New-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/new-mapivirtualdirectory)

- [Remove-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/remove-mapivirtualdirectory)
