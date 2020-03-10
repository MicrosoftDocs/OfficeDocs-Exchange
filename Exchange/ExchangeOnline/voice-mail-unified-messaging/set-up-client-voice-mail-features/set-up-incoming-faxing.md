---
localization_priority: Normal
description: Admin can learn how to setup inbound faxing in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5d3cae58-1690-424d-9bef-011911d0b608
ms.reviewer: 
f1.keywords:
- NOCSH
title: Setting up incoming faxing in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Setting up incoming faxing in Exchange Online

Microsoft Exchange Unified Messaging (UM) relies on certified fax partner solutions for enhanced fax features such as outbound fax or fax routing. By default, Exchange Online isn't configured to allow incoming faxes to be delivered to a user that's enabled for UM. Instead, an Exchange Online redirects incoming fax calls to a certified fax partner solution. The fax partner's server receives the fax data and then sends it to the user's mailbox in an email message with the fax included as a .tif attachment.

For more information about fax partners, see [Microsoft Solution Providers for Fax Partners](https://go.microsoft.com/fwlink/p/?LinkId=190238).

## Deploying and configuring faxing
<a name="deployandconfigure"> </a>

 UM forwards incoming fax calls to a dedicated fax partner solution, which then establishes the fax call with the fax sender and receives the fax on behalf of the UM-enabled user. However, to allow UM-enabled users to receive fax messages in their mailboxes, you must first enable incoming faxing and set the fax partner's URI on the UM mailbox policy that's linked to the UM-enabled user or users. You can allow or prevent incoming faxing on UM dial plans, UM mailbox policies, and on the mailbox for a UM-enabled user. For details, see the following topics:

- [Allow users in the same dial plan to receive faxes](allow-users-in-the-same-dial-plan-to-receive-faxes.md)

- [Prevent users in the same dial plan from receiving faxes](prevent-users-in-the-same-dial-plan-from-receiving-faxes.md)

- [Enable faxing for a group of users](enable-faxing-for-a-group-of-users.md)

- [Disable faxing for a group of users](disable-faxing-for-a-group-of-users.md)

- [Enable a user to receive faxes](enable-a-user-to-receive-faxes.md)

- [Prevent a user from receiving faxes](prevent-a-user-from-receiving-faxes.md)

### Step 1: Deploy Unified Messaging
<a name="step1deployUM"> </a>

Before you can set up faxing for your organization, you need to successfully configure your supported Voice over IP (VoIP) gateways to allow faxing. For more information, see [Voice mail in Exchange Online: Unified Messaging](../voice-mail-unified-messaging.md). For details about how to deploy VoIP gateways and IP Private Branch eXchanges (PBXs), see [Connect your voice mail system to your telephone network in Exchange Online](../connect-voice-mail-system/connect-voice-mail-system.md).

> [!IMPORTANT]
> Sending and receiving faxes using T.38 or G.711 isn't supported in an environment where Unified Messaging and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server are integrated.

### Step 2: Configure fax partner servers
<a name="step2configurefax"> </a>

Next, you need to enable incoming faxing and configure the fax partner's URI on each UM mailbox policy that you require in your organization. To successfully deploy incoming faxing, you must integrate a certified fax partner solution with Exchange Unified Messaging. For details, see [Fax advisor for Exchange UM](fax-advisor-for-exchange-um.md). For a list of certified fax partners, see [Microsoft Solution Providers for Fax Partners](https://go.microsoft.com/fwlink/p/?LinkId=190238)

> [!NOTE]
> Because the fax partner server is external to your organization, firewall ports must be configured to allow the T.38 protocol ports that enable faxing over an IP-based network. By default, the T.38 protocol uses TCP port 6004. It can also use User Datagram Protocol (UDP) port 6044, but this will be defined by the hardware manufacturer. The firewall ports must be configured to allow fax data that uses the TCP or UDP ports or port ranges defined by the manufacturer.

### Step 3: Enable faxing on Unified Messaging
<a name="step3configurefaxing"> </a>

Three components must be configured correctly for users to be able to receive faxes by using Unified Messaging:

- UM dial plans

- UM mailbox policies

- UM mailboxes

Faxing can be enabled or disabled on UM dial plans, UM mailbox policies, or on an individual UM-enabled user's mailbox. UM mailbox policies can be enabled or disabled for faxing using either the Exchange admin center (EAC) or Exchange Online PowerShell. Enabling and disabling of dial plans and individual UM-enabled users needs to be done using Exchange Online PowerShell. The following table shows the options that are available and the cmdlets and parameters that are used for enabling and disabling faxing.

|**UM component**|**Enable/disable using the EAC?**|**Exchange Online PowerShell example for enabling faxing**|
|:-----|:-----|:-----|
|Dial plan|No|`Set-UMDialPlan -Identity MyUMDialPlan -faxenabled $true`|
|UM mailbox policy|Yes|`Set-UMMaiboxPolicy -Identity MyPolicy -AllowFax $true`|
|UM-enabled user|No|`Set-UMMailbox -Identity tonysmith -faxenabled $true`|

By default, although the UM dial plan and the user's mailbox allow incoming faxes, you must first enable inbound faxing on the UM mailbox policy that's assigned to the UM-enabled user and then enter the fax partner server's URI.

To enable UM-enabled users to receive faxes, you must do the following:

- Verify that each UM dial plan allows the users who are associated with the dial plan to receive faxes. By default, all users who are associated with a dial plan can receive faxes. For UM-enabled users to receive fax messages in their mailbox, each VoIP gateway or IP PBX must be configured to accept incoming fax calls. You must also enable fax messages to be received by users who are linked with the dial plan. For more information about how to enable users linked with a dial plan to receive faxes or to prevent them from doing this, see [Enable a user to receive faxes](enable-a-user-to-receive-faxes.md).

    > [!NOTE]
    > If you prevent fax messages from being received on a dial plan, no users who are associated with the dial plan will be able to receive faxes, even if you configure an individual user's properties to allow them to receive faxes. Enabling or disabling faxing on a UM dial plan takes precedence over the settings for an individual UM-enabled user.

- Configure the UM mailbox policy that's associated with the UM-enabled user. The UM mailbox policy must be configured to allow incoming faxes, including the fax partner's URI and the name of the fax partner's server. The _FaxServerURI_ parameter must use the following form: sip:\<_fax server URI_\>:\<_port_\>;\<_transport_\>, where "fax server URI" is either a fully qualified domain name (FQDN) or an IP address of the fax partner server. The "port" is the port on which the fax server listens for incoming fax calls and "transport" is the transport protocol that's used for the incoming fax (UDP, TCP, or Transport Layer Security (TLS)). For example, you might configure a UM mailbox policy to receive a fax as follows.

  ```PowerShell
  Set-UMMailboxPolicy MyUMMailboxPolicy -AllowFax $true -FaxServerURI "sip:faxserver.abc.com:5060;transport=tcp"
  ```

- For details, see [Set the partner fax server URI to allow faxing](set-the-partner-fax-server-uri-to-allow-faxing.md).

    > [!CAUTION]
    > Although you can include multiple entries in the format for the _FaxServerURI_ by separating them with a semicolon, only one entry will be used. This parameter allows only one entry to be used, and adding multiple entries won't enable you to load balance fax requests.

- Verify that the mailbox that's UM-enabled can receive fax messages. By default, all users who are associated with a dial plan can receive faxes. However, there may be situations when a user can't receive faxes because the ability to receive faxes has been disabled on their mailbox. For more information about how to enable a UM-enabled user to receive faxes, see [Enable a user to receive faxes](enable-a-user-to-receive-faxes.md).

    You can prevent an individual user who's associated with a dial plan from receiving fax messages. To do this, configure the properties for the user by using the **Set-UMMailbox** cmdlet in Exchange Online PowerShell. You can also use the **Set-UMMailboxPolicy** cmdlet to prevent multiple users from receiving fax messages. For more information about how to prevent a user or users from receiving fax messages, see [Prevent a user from receiving faxes](prevent-a-user-from-receiving-faxes.md).

### Step 4: Configure authentication
<a name="step4configureauthentication"> </a>

In addition to configuring your UM dial plans, UM mailbox policies, and UM-enabled users, you have to configure authentication between Exchange Online and the fax partner server. Exchange Online must be able to authenticate the origin of the messages that claim to be coming from the fax partner server. Any unauthenticated messages claiming to have come from a fax partner server won't be processed by Exchange Online.

To authenticate the connection from the fax partner server to Exchange Online, you can use:

- Mutual TLS

- Sender ID validation

- A dedicated Inbound connector

An Inbound connector should be sufficient for authenticating the fax partner servers deployed in your organization. The connector will ensure that Exchange Online treats all traffic coming from the fax partner server as authenticated.

For details, see [Configure mail flow using connectors in Office 365](../../mail-flow-best-practices/use-connectors-to-configure-mail-flow/use-connectors-to-configure-mail-flow.md).

If the fax partner server hosted is in the cloud, it's a good idea to authenticate the fax partner server using a sender ID check. This type of authentication ensures that the IP address that the fax message came from is authorized to send email messages on behalf of the fax partner domain that the message claims to have come from. DNS is used to store the sender ID records (or sender policy framework (SPF) records) and fax partners must publish their SPF records in the DNS forward lookup zone. Exchange will validate the IP addresses by querying DNS. However, the sender ID agent must be running on a Mailbox server to be able to perform the DNS query.

You can also use TLS to encrypt the network traffic, or mutual TLS for encryption and authentication between the fax partner server and Exchange Online.
