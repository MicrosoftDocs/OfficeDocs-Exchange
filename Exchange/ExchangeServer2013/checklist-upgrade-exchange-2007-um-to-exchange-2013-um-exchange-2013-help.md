---
title: 'Checklist: Upgrade Exchange 2007 UM to Exchange 2013 UM: Exchange 2013 Help'
TOCTitle: 'Checklist: Upgrade Exchange 2007 UM to Exchange 2013 UM'
ms:assetid: 99b1a081-4052-4516-b63c-77622cbdf962
ms:mtpsurl: https://technet.microsoft.com/library/Dn169229(v=EXCHG.150)
ms:contentKeyID: 53382780
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Checklist: Upgrade Exchange 2007 UM to Exchange 2013 UM

_**Applies to:** Exchange Server 2013_

Use this checklist to help you upgrade Exchange 2007 Unified Messaging (UM) to Exchange 2013 UM. Be sure to refer to this information when you're upgrading your Exchange 2007 organization and your UM deployment to Exchange 2013. For step-by-step instructions for upgrading to Exchange 2013 UM, see [Upgrade Exchange 2007 UM to Exchange 2013 UM](upgrade-exchange-2007-um-to-exchange-2013-um-exchange-2013-help.md).

Before you start working with this checklist, make sure you're familiar with the concepts in:

  - [Planning for Unified Messaging](planning-for-unified-messaging-exchange-2013-help.md)

  - [Telephone system integration with UM](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephone-system-integration-with-um)

  - [Connect your voice mail system to your telephone network](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/connect-voice-mail-system)

For step-by-step guidance about how to upgrade from Exchange 2010 UM to Exchange 2013 UM, see [Checklist: Upgrade Exchange 2010 UM to Exchange 2013 UM](checklist-upgrade-exchange-2010-um-to-exchange-2013-um-exchange-2013-help.md).

## Checklist for upgrading Exchange 2007 UM to Exchange 2013 UM

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Done?</th>
<th>Tasks</th>
<th>Topic</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p></p></td>
<td><p>Deploy and configure telephony components</p></td>
<td><p><a href="connect-um-to-your-telephone-system-exchange-2013-help.md">Connect UM to your telephone system</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Review the system requirements before installing Exchange 2013</p></td>
<td><p><a href="exchange-2013-system-requirements-exchange-2013-help.md">Exchange 2013 system requirements</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Verify that you meet the prerequisites for installation</p></td>
<td><p><a href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Install the required Client Access and Mailbox servers</p></td>
<td><p><a href="install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md">Install Exchange 2013 using the Setup wizard</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Verify the installation and review the server setup logs</p></td>
<td><p><a href="verify-an-exchange-2013-installation-exchange-2013-help.md">Verify an Exchange 2013 installation</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>If required, install the required UM language packs</p></td>
<td><p><a href="install-a-um-language-pack-exchange-2013-help.md">Install a UM language pack</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Import dial plan and auto attendant custom prompts</p></td>
<td><p><a href="import-custom-prompts-from-exchange-2007-to-exchange-2013-exchange-2013-help.md">Import custom prompts from Exchange 2007 to Exchange 2013</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Export and import certificates</p></td>
<td><p><a href="deploying-certificates-for-um-exchange-2013-help.md">Deploying certificates for UM</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Configure the UM startup mode on all Exchange 2013 Client Access servers</p></td>
<td><p><a href="configure-the-startup-mode-on-a-client-access-server-exchange-2013-help.md">Configure the startup mode on a Client Access server</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Configure the UM startup mode on all Exchange 2013 Mailbox servers</p></td>
<td><p><a href="configure-the-startup-mode-on-a-mailbox-server-exchange-2013-help.md">Configure the startup mode on a Mailbox server</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Create or configure existing UM dial plans</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan">Create a UM dial plan</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/manage-um-dial-plan">Manage a UM dial plan</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Create or configure existing UM IP gateways</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway">Create a UM IP gateway</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/manage-um-ip-gateway">Manage a UM IP gateway</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Create a UM hunt group</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-hunt-group">Create a UM hunt group</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Create or configure UM auto attendants</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/create-a-um-auto-attendant">Create a UM auto attendant</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/manage-um-auto-attendant">Manage a UM auto attendant</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Create or configure UM mailbox policies</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy">Create a UM mailbox policy</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy">Manage a UM mailbox policy</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Move existing UM-enabled mailboxes to Exchange 2013</p></td>
<td><p><a href="mailbox-moves-in-exchange-2013-exchange-2013-help.md">Mailbox moves in Exchange 2013</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Enable new users for UM or configure settings for an existing UM-enabled user</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail">Enable a user for voice mail</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/manage-voice-mail-settings">Manage voice mail settings for a user</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Configure your VoIP gateways, IP PBXs, and SIP-enabled PBXs to send all incoming calls to the Exchange 2013 Client Access servers</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-voip-gateways">Configuration notes for supported VoIP gateways, IP PBXs, and PBXs</a></p>
<p><a href="connect-a-voip-gateway-ip-pbx-or-session-border-controller-to-um-exchange-2013-help.md">Connect a VoIP gateway, IP PBX, or session border controller to UM</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Disable call answering on the Exchange 2007 UM servers</p></td>
<td><p><a href="https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb123529(v=exchg.80)">How to Disable Unified Messaging on Exchange 2007</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Remove an Exchange 2007 UM server from a dial plan</p></td>
<td><p><a href="https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa997238(v=exchg.80)">How to Remove a Unified Messaging Server from a Dial Plan</a></p></td>
</tr>
</tbody>
</table>
