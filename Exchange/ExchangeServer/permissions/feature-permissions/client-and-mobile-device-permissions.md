---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to perform tasks for clients and mobile devices in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 57eca42a-5a7f-4c65-89f0-7a84f2dbea19
ms.reviewer:
title: Clients and mobile devices permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Clients and mobile devices permissions

The permissions required to perform tasks for clients and mobile devices vary depending on the procedure being performed or the cmdlet you want to run. For more information about client and mobile device features, see [Clients and mobile](../../clients/clients.md).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

> [!NOTE]
> Some features may require that you have local administrator permissions on the server you want to manage. To manage these features, you must be a member of the Local Administrators group on that server.

## Client Access service permissions
<a name="ClientAccessServer"> </a>

You can configure any of the following features for the Client Access service.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Client Access service array settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Client Access service settings|[Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Client Access service email channel settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Client Access user settings|[Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Client Access virtual directory settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|RPC Client Access settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|Push notification proxy settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|OAuth authentication redirection settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|

## Exchange ActiveSync permissions
<a name="ExchangeActiveSync"> </a>

You can configure any of the following for Exchange ActiveSync.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Exchange ActiveSync Autoblock settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Exchange ActiveSync mailbox policy settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange ActiveSync server settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange ActiveSync settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange ActiveSync user settings|[Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Exchange ActiveSync virtual directory settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Mobile device mailbox policy settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Mobile device user settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|

## Autodiscover permissions
<a name="Autodiscover"> </a>

You can configure the following for the Autodiscover service.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Autodiscover service configuration settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Delegated Setup](https://docs.microsoft.com/exchange/delegated-setup-exchange-2013-help) <br/> [Hygiene Management](https://docs.microsoft.com/exchange/hygiene-management-exchange-2013-help)|
|Autodiscover virtual directory settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|

## Availability service permissions
<a name="AvailabilityService"> </a>

You can configure the following for the Availability service.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Availability service address space settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|Availability service configuration settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|

## Client throttling permissions
<a name="ClientThrottling"> </a>

You can configure the following for client throttling.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Client throttling settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|

## Exchange Web Services permissions
<a name="ClientThrottling"> </a>

You can configure the following for Web Services virtual directories.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Exchange Web Services virtual directory settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Test Exchange Web Services|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Test Outlook Web Services|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|

## Outlook Anywhere permissions
<a name="OutlookAnywhere"> </a>

You can configure and manage the following settings for Outlook Anywhere.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Outlook Anywhere configuration (enable, disable, change, view)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Delegated Setup](https://docs.microsoft.com/exchange/delegated-setup-exchange-2013-help) <br/> [Hygiene Management](https://docs.microsoft.com/exchange/hygiene-management-exchange-2013-help)|
|RPC over HTTP Proxy component|Local Server Administrator|
|Test Outlook Anywhere connectivity|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|

## Outlook on the web permissions
<a name="OutlookWebApp"> </a>

You can use the following features to view Outlook on the web settings, control security and user access to Outlook on the web, and test Outlook on the web connectivity.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Graphics editor|Local Server Administrator|
|IIS Manager|Local Server Administrator|
|ISA Server 2006|ISA Server Enterprise Administrator|
|Outlook on the web mailbox policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Outlook on the web virtual directories|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Registry Editor|Local Server Administrator|
|S/MIME configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Text editor|Local Server Administrator|
|View Outlook on the web mailbox policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Delegated Setup](https://docs.microsoft.com/exchange/delegated-setup-exchange-2013-help) <br/> [Hygiene Management](https://docs.microsoft.com/exchange/hygiene-management-exchange-2013-help)|

## POP3 and IMAP4 permissions
<a name="POP3andIMAP4"> </a>

You can configure the following for POP3 and IMAP4.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|IMAP4 settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|POP3 settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|Test IMAP4 settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|Test POP3 settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|

## Windows PowerShell virtual directory permissions
<a name="PowerShell"> </a>

You can configure the following for Windows PowerShell.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Test Windows PowerShell|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Windows PowerShell settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|

## Text Messaging permissions
<a name="TextMessaging"> </a>

You can configure the following for text messaging.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Text messaging notification settings|[Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Text messaging settings|[Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Text messaging user settings|[Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
