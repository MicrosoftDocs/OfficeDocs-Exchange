---
title: 'Checklist: Integrate Exchange 2013 UM with Lync Server: Exchange 2013 Help'
TOCTitle: 'Checklist: Integrate Exchange 2013 UM with Lync Server'
ms:assetid: 3b82e86f-9f30-4445-96ad-744082abeaeb
ms:mtpsurl: https://technet.microsoft.com/library/Dd638120(v=EXCHG.150)
ms:contentKeyID: 49315388
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Checklist: Integrate Exchange 2013 UM with Lync Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Use this checklist to install and deploy Unified Messaging (UM) and Microsoft Lync Server 2013. In this topic, "Lync Server" also refers to Lync Server 2010. However, Microsoft Office Communications Server 2007 R2 can also be deployed together with Unified Messaging.

Before you start working with this checklist, make sure you're familiar with the concepts in:

  - [Deploying Exchange 2013 UM and Lync Server overview](deploying-exchange-2013-um-and-lync-server-overview-exchange-2013-help.md)

  - [Coexistence with Office Communications Server 2007 R2 and Lync Server](coexistence-with-office-communications-server-2007-r2-and-lync-server-exchange-2013-help.md)

For more information about how to perform the tasks that must be completed for Lync Server, see [Microsoft Lync Server 2013](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013).

## Checklist for deploying Microsoft Lync Server and Unified Messaging

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
<td><p> </p></td>
<td><p>Review the system requirements before installing Exchange Server 2013.</p></td>
<td><p><a href="exchange-2013-system-requirements-exchange-2013-help.md">Exchange 2013 system requirements</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>Verify that you meet the prerequisites for installation.</p></td>
<td><p><a href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Review the prerequisites for integrating Microsoft Lync Server 2013 and Microsoft Exchange Server 2013.</p></td>
<td><p><a href="https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/integrate-with-exchange/integrate-with-exchange">Prerequisites for Integrating Microsoft Lync Server 2013 and Microsoft Exchange Server 2013</a></p>

> [!TIP]
> The Unified Communications Managed API (UCMA) 4.0 Runtime is required for Exchange 2013 and Lync Server 2010 and 2013 and is installed during installation. To download and review information about UCMA 4.0, see <A href="https://www.microsoft.com/download/details.aspx?id=34992">Unified Communications Managed API 4.0 Runtime</A>

</td>
</tr>
<tr class="even">
<td><p><strong> </strong></p></td>
<td><p>Install the required Client Access and Mailbox servers.</p></td>
<td><p><a href="install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md">Install Exchange 2013 using the Setup wizard</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Verify the installation and review the server setup logs.</p></td>
<td><p><a href="verify-an-exchange-2013-installation-exchange-2013-help.md">Verify an Exchange 2013 installation</a></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>If required, install the required UM language packs.</p></td>
<td><p><a href="install-a-um-language-pack-exchange-2013-help.md">Install a UM language pack</a></p></td>
</tr>
<tr class="odd">
<td><p><strong> </strong></p></td>
<td><p>Create the number of SIP URI dial plans required for your organization.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan">Create a UM dial plan</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Configure the dial plan security setting.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/configure-voip-security-setting">Configure the VoIP security setting</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Configure the number of concurrent calls on your Mailbox servers.</p></td>
<td><p><a href="configure-the-number-of-incoming-calls-on-a-mailbox-server-exchange-2013-help.md">Configure the number of incoming calls on a Mailbox server</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Configure Outlook Voice Access numbers and other settings.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/manage-um-dial-plan">Manage a UM dial plan</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Add all Client Access and Mailbox servers to each SIP URI dial plan.</p></td>
<td><p><a href="add-mailbox-and-client-access-servers-to-a-sip-uri-dial-plan-exchange-2013-help.md">Add Mailbox and Client Access servers to a SIP URI dial plan</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Configure outbound dialing for Unified Messaging. Allow all calls on the SIP URI dial plans and UM mailbox policies that are linked to those dial plans.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/authorize-calls-for-users-in-a-dial-plan">Authorize calls for users in a dial plan</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/authorize-calls-for-a-group-of-users">Authorize calls for a group of users</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Create the required number of auto attendants.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/create-a-um-auto-attendant">Create a UM auto attendant</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Set up and configure each of the UM auto attendants.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/set-up-um-auto-attendant">Set up a UM auto attendant</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Create, import, and enable a new Exchange certificate for UM or enable a mutually-trusted third-party certificate.</p></td>
<td><p><a href="add-mailbox-and-client-access-servers-to-a-sip-uri-dial-plan-exchange-2013-help.md">Add Mailbox and Client Access servers to a SIP URI dial plan</a></p></td>
</tr>
<tr class="even">
<td><p><strong> </strong></p></td>
<td><p>Configure the UM startup mode to Dual or TLS for each Client Access and Mailbox server.</p></td>
<td><p><a href="configure-the-startup-mode-on-a-mailbox-server-exchange-2013-help.md">Configure the startup mode on a Mailbox server</a></p>
<p><a href="configure-the-startup-mode-on-a-client-access-server-exchange-2013-help.md">Configure the startup mode on a Client Access server</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Restart the Microsoft Exchange Unified Messaging service and the Unified Messaging Call Router service on all Exchange servers to load the required certificates.</p></td>
<td><p><a href="stop-the-microsoft-exchange-unified-messaging-service-exchange-2013-help.md">Stop the Microsoft Exchange Unified Messaging service</a></p>
<p><a href="start-the-microsoft-exchange-unified-messaging-service-exchange-2013-help.md">Start the Microsoft Exchange Unified Messaging service</a></p>
<p><a href="stop-the-microsoft-exchange-unified-messaging-call-router-service-exchange-2013-help.md">Stop the Microsoft Exchange Unified Messaging Call Router service</a></p>
<p><a href="start-the-microsoft-exchange-unified-messaging-call-router-service-exchange-2013-help.md">Start the Microsoft Exchange Unified Messaging Call Router service</a></p></td>
</tr>
<tr class="even">
<td><p><strong> </strong></p></td>
<td><p>Create a UM mailbox policy or configure the default UM mailbox policy.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy">Create a UM mailbox policy</a></p>
<p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy">Manage a UM mailbox policy</a></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Enable users for Unified Messaging with a SIP address and link them to a SIP URI dial plan.</p></td>
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail">Enable a user for voice mail</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Review the Lync Server 2013 Planning documentation.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-planning">Planning</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Install and deploy Lync Server 2013.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-deploying-lync-server">Deploying Lync Server 2013</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Import the mutually-trusted internal PKI or third-party certificate that is imported on the Exchange UM servers.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-configure-certificates-for-servers">Configure Certificates for Servers</a></p>
<p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-configure-certificates-on-the-server-running-microsoft-exchange-server-unified-messaging">Configure Certificates on the Server Running Microsoft Exchange Server Unified Messaging</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>If required, start Lync services on servers to load the certificates.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-start-services-on-servers">Start Services on Servers</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Open the Exchange Management Shell and run the exchucutil.ps1 script located in the %Program Files%\Microsoft\Exchange Server\V15\Scripts folder.</p>
<p></p></td>
<td><p><a href="configure-um-to-work-with-lync-server-exchange-2013-help.md">Configure UM to work with Lync Server</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Review the requirements for Enterprise Voice.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-software-prerequisites-for-enterprise-voice">Software Prerequisites for Enterprise Voice</a></p>
<p><a href="https://docs.microsoft.com/skypeforbusiness/deploy/deploy-enterprise-voice/enterprise-voice-security">Security and Configuration Prerequisites for Enterprise Voice</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Deploy and configure media gateways or Mediation Servers and define peers.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-deploying-mediation-servers-and-defining-peers">Deploying Mediation Servers and Defining Peers</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Configure a trunk between a Mediation Server and one or more of the peers to provide public switched telephone network (PSTN) connectivity.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-configuring-trunks">Configuring Trunks</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Create and configure a Lync dial plan and create, define, and associate normalization rules.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-configuring-dial-plans">Configuring Dial Plans</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Configure voice policies and define telephone usage and outbound call routes.</p></td>
<td><p><a href="https://docs.microsoft.com/skypeforbusiness/deploy/deploy-enterprise-voice/voice-and-pstn">Configuring Voice Policies, PSTN Usage Records, and Voice Routes</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Run the Exchange Integration utility (ocsumutil.exe), which creates the contact objects for Outlook Voice Access and for the auto attendants.</p></td>
<td><p><a href="https://docs.microsoft.com/lyncserver/lync-server-2013-configure-lync-server-2013-to-work-with-unified-messaging-on-microsoft-exchange-server">Configure Lync Server 2013 to Work with Unified Messaging on Microsoft Exchange Server</a></p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p>Define, deploy, and configure any required advanced Enterprise Voice features.</p></td>
<td><p><a href="https://docs.microsoft.com/skypeforbusiness/deploy/deploy-enterprise-voice/deploy-advanced-enterprise-voice-features">Deploying Advanced Enterprise Voice Features</a></p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p>Enable the users for Enterprise Voice. Enter a line URI and assign a voice policy and a Lync dial plan.</p></td>
<td><p><a href="https://docs.microsoft.com/skypeforbusiness/deploy/deploy-enterprise-voice/enable-users-for-enterprise-voice">Enable Users for Enterprise Voice</a></p></td>
</tr>
</tbody>
</table>
