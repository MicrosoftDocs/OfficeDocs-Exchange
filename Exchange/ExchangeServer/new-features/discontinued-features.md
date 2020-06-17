---
localization_priority: Priority
description: This topic discusses the components, features, or functionality that have been removed, discontinued, or replaced in Exchange Server 2016 and Exchange Server 2019.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 0ac0001c-b314-4108-b895-d9c0e271b489
monikerRange: exchserver-2016 || exchserver-2019
title: What's discontinued in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.reviewer:
ms.prod: exchange-server-it-pro
manager: serdars

---

# What's discontinued in Exchange Server

::: moniker range="exchserver-2019"
This topic discusses the components, features, and functionality that's been removed, discontinued, or replaced in Exchange 2019.

## Discontinued features from Exchange 2016 to Exchange 2019

This section lists the Exchange 2016 features that are no longer available in Exchange 2019.

### Architecture

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Unified Messaging (UM)|Unified Messaging has been removed from Exchange 2019. We recommend that Exchange 2019 organizations transition to Skype for Business Cloud Voice Mail.|

## Discontinued features from Exchange 2013 to Exchange 2019

This section lists the Exchange 2013 features that are no longer available in Exchange 2019.

### Architecture

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Unified Messaging (UM)|Unified Messaging has been removed from Exchange 2019. We recommend that Exchange 2019 organizations transition to Skype for Business Cloud Voice Mail.|
|Client Access server role|The Client Access server role has been replaced by Client Access services that run on the Mailbox server role. The Mailbox server role now performs all functionality that was previously included with the Client Access server role. For more information about the new Mailbox server role, see [Exchange Server architecture](../architecture/architecture.md).|
|MAPI/CDO library|The MAPI/CDO library has been replaced by Exchange Web Services (EWS), Exchange ActiveSync (EAS), and Representational State Transfer (REST)<sup>\*</sup> APIs. If an application uses the MAPI/CDO library, it needs to move to EWS, EAS, or the REST APIs to communicate with Exchange 2019.|

<sup>\*</sup> REST APIs will be included in a future release of Exchange 2019.

## De-emphasized features in Exchange 2019

The following features are being de-emphasized in Exchange 2019 and may not be included in future versions of Exchange.

- Third-party replication APIs

- RPC over HTTP

- Database availability group (DAG) support for failover cluster administrative access points
::: moniker-end

::: moniker range="exchserver-2016"
This topic discusses the components, features, or functionality that have been removed, discontinued, or replaced in Exchange 2016.

## Discontinued features from Exchange 2013 to Exchange 2016

This section lists the Exchange 2013 features that are no longer available in Exchange 2016.

### Architecture

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Client Access server role|The Client Access server role has been replaced by Client Access services that run on the Mailbox server role. The Mailbox server role now performs all functionality that was previously included with the Client Access server role. For more information about the new Mailbox server role, see [Exchange Server architecture](../architecture/architecture.md).|
|MAPI/CDO library|The MAPI/CDO library has been replaced by Exchange Web Services (EWS), Exchange ActiveSync (EAS), and Representational State Transfer (REST)<sup>\*</sup> APIs. If an application uses the MAPI/CDO library, it needs to move to EWS, EAS, or the REST APIs to communicate with Exchange 2016.|

<sup>\*</sup> REST APIs will be included in a future release of Exchange 2016.

## De-emphasized features in Exchange 2016

The following features are being de-emphasized in Exchange 2016 and may not be included in future versions of Exchange.

- Third-party replication APIs

- RPC over HTTP

- Database Availability Group support for failover cluster administrative access points

## Discontinued features from Exchange 2010 to Exchange 2016

This section lists the Exchange 2010 features that are no longer available in Exchange 2016.

### Architecture

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Hub Transport server role|The Hub Transport server role has been replaced by Transport services which run on the Mailbox server role. The Mailbox server role includes the Microsoft Exchange Transport, Microsoft Exchange Mailbox Transport Delivery, the Microsoft Exchange Mailbox Transport Submission, and the Microsoft Exchange Frontend Transport service. For more information, see [Mail flow and the transport pipeline](../mail-flow/mail-flow.md).|
|Unified Messaging server role|The Unified Messaging server role has been replaced by Unified Messaging services which run on the Mailbox and Client Access server roles. The Mailbox server role includes the Microsoft Exchange Unified Messaging service and the Client Access server role includes the Microsoft Exchange Unified Messaging Call Router service. For more information, see [Voice Architecture Changes](https://docs.microsoft.com/exchange/voice-architecture-changes-exchange-2013-help).|
|MAPI/CDO library|The MAPI/CDO library has been replaced by Exchange Web Services (EWS), Exchange ActiveSync (EAS), and Representational State Transfer (REST)<sup>\*</sup> APIs. If an application uses the MAPI/CDO library, it needs to move to EWS, EAS, or the REST APIs to communicate with Exchange 2016.|

<sup>\*</sup> REST APIs will be included in a future release of Exchange 2016.

### Management interfaces

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Exchange Management Console and Exchange Control Panel|The Exchange Management Console and the Exchange Control Panel have been replaced by the Exchange admin center (EAC). EAC uses the same virtual directory (/ecp) as the Exchange Control Panel. For more information, see [Exchange admin center in Exchange Server](../architecture/client-access/exchange-admin-center.md).|

### Client access

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Outlook 2003 is not supported|To connect Microsoft Outlook to Exchange 2016, the use of the Autodiscover service is required. However, Microsoft Outlook 2003 doesn't support the use of the Autodiscover service.|
|RPC/TCP access for Outlook clients|In Exchange 2016, Microsoft Outlook clients can connect using Outlook Anywhere (RPC/HTTP) or MAPI over HTTP Outlook 2013 Service Pack 1 and later. If you have Outlook clients in your organization, using Outlook Anywhere and/or MAPI over HTTP is required. For more information, see [Outlook Anywhere](https://docs.microsoft.com/exchange/outlook-anywhere-exchange-2013-help) and [MAPI over HTTP in Exchange Server](../clients/mapi-over-http/mapi-over-http.md).|

### Outlook Web App and Outlook

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Spell check|Outlook Web App no longer has built-in spell check services. Instead, it uses the spell check features in your Web browsers.|
|Customizable filters|Outlook Web App no longer has customizable filtered views and no longer supports saving filtered views to Favorites. Customizable filters have been replaced by fixed filters that can be used to view all messages, unread messages, messages sent to the user, or flagged messages.|
|Message flags|The ability to set a custom date on a message flag isn't available in Outlook Web App. You can use Outlook to set custom dates.|
|Chat contact list|The chat contact list that appeared in the folder list in Outlook Web App for Exchange 2010 is no longer available.|
|Search folders|The ability for users to use Search folders isn't currently available in Outlook Web App.|
|Web Parts|Outlook on the web no longer includes support for Web Parts. Customers will need to develop replacement functionality to meet this need in their environments.|

### Mail flow

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Linked connectors|The ability to link a Send connector to a Receive connector has been removed. Specifically, the _LinkedReceiveConnector_ parameter has been removed from [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/new-sendconnector) and [Set-SendConnector](https://docs.microsoft.com/powershell/module/exchange/set-sendconnector).|

### Antispam and antimalware

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Antispam agent management in the EMC|In Exchange 2010, when you enabled the antispam agents on a Hub Transport server, you could manage the antispam agents in the Exchange Management Console (EMC). In Exchange 2016, when you enable the antispam agents on a Mailbox server, you can't manage the agents using the EAC. You can only use the Exchange Management Shell. For information about how to enable the antispam agents on a Mailbox server, see [Enable antispam functionality on Mailbox servers](../antispam-and-antimalware/antispam-protection/antispam-on-mailbox-servers.md).|
|Connection Filtering agent on Hub Transport servers|In Exchange 2010, when you enabled the antispam agents on a Hub Transport server, the Attachment Filter agent was the only antispam agent that wasn't available. In Exchange 2016, when you enable the antispam agents on a Mailbox server, the Attachment Filter agent and the Connection Filtering agent aren't available. The Connection Filtering agent provides IP Allow List and IP Block List capabilities. For information about how to enable the antispam agents on a Mailbox server, see [Enable antispam functionality on Mailbox servers](../antispam-and-antimalware/antispam-protection/antispam-on-mailbox-servers.md).  <br/> **Note**: The only way to enable the Connection Filtering agent is to install an Edge Transport server in the perimeter network. For more information, see [Edge Transport servers](../architecture/edge-transport-servers/edge-transport-servers.md).|

### Messaging policy and compliance

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Managed Folders|In Exchange 2010, you use managed folders for messaging retention management (MRM). In Exchange 2016, managed folders aren't supported. You must use retention policies for MRM.  <br/> **Note**: Cmdlets related to managed folders are still available. You can create managed folders, managed content settings and managed folder mailbox policies, and apply a managed folder mailbox policy to a user, but the MRM assistant skips processing of mailboxes that have a managed folder mailbox policy applied.|
|Port Managed Folder wizard|In Exchange 2010, you use the Port Managed Folder wizard to create retention tags based on managed folder and managed content settings. In Exchange 2016, the Exchange admin center doesn't include this functionality. You can use the **New-RetentionPolicyTag** cmdlet with the _ManagedFolderToUpgrade_ parameter to create a retention tag based on a managed folder.|

### Unified Messaging and voice mail

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|Directory lookups using Automatic Speech Recognition (ASR)|In Exchange 2010, Outlook Voice Access users can use speech inputs using Automatic Speech Recognition (ASR) to search for users listed in the directory. Speech inputs could be also used in Outlook Voice Access to navigate menus, messages, and other options. However, even if an Outlook Voice Access user is able to use speech inputs, they have to use the telephone key pad to enter their PIN, and navigate personal options.  <br/> In Exchange 2016, authenticated and non-authenticated Outlook Voice Access users can't search for users in the directory using speech inputs or ASR in any language. However, callers that call into an auto attendant can use speech inputs in multiple languages to navigate auto attendant menus and search for users in the directory.|

### Mailbox database copies

|**Feature**|**Comments and mitigation**|
|:-----|:-----|
|**Update-MailboxDatabaseCopy** <br/> Update Mailbox Database Copy wizard|Content index catalog seeding is no longer possible over the replication network; it can only be done over a MAPI network. This is true even when you use the `-Network` parameter in the **Update-MailboxDatabaseCopy** cmdlet.|
::: moniker-end
