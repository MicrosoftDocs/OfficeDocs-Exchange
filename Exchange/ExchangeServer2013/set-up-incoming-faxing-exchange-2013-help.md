---
title: 'Setting up incoming faxing: Exchange 2013 Help'
TOCTitle: Setting up incoming faxing
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 5d3cae58-1690-424d-9bef-011911d0b608
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Setting up incoming faxing in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Microsoft Exchange Unified Messaging (UM) relies on certified fax partner solutions for enhanced fax features such as outbound fax or fax routing. By default, Exchange servers aren't configured to allow incoming faxes to be delivered to a user that's enabled for UM. Instead, an Exchange server redirects incoming fax calls to a certified fax partner solution. The fax partner's server receives the fax data and then sends it to the user's mailbox in an email message with the fax included as a .tif attachment.

## Deploying and configuring faxing
<a name="deployandconfigure"> </a>

 UM forwards incoming fax calls to a dedicated fax partner solution, which then establishes the fax call with the fax sender and receives the fax on behalf of the UM-enabled user. However, to allow UM-enabled users to receive fax messages in their mailboxes, you must first enable incoming faxing and set the fax partner's URI on the UM mailbox policy that's linked to the UM-enabled user or users. You can allow or prevent incoming faxing on UM dial plans, UM mailbox policies, and on the mailbox for a UM-enabled user. For details, see the following topics:

- [Allow users in the same dial plan to receive faxes](allow-users-in-the-same-dial-plan-to-receive-faxes-exchange-2013-help.md)

- [Prevent users in the same dial plan from receiving faxes](prevent-users-in-the-same-dial-plan-from-receiving-faxes-exchange-2013-help.md)

- [Enable faxing for a group of users](enable-faxing-for-a-group-of-users-exchange-2013-help.md)

- [Disable faxing for a group of users](disable-faxing-for-a-group-of-users-exchange-2013-help.md)

- [Enable a user to receive faxes](enable-a-user-to-receive-faxes-exchange-2013-help.md)

- [Prevent a user from receiving faxes](prevent-a-user-from-receiving-faxes-exchange-2013-help.md)

### Step 1: Deploy Unified Messaging
<a name="step1deployUM"> </a>

Before you can set up faxing for your on-premises or hybrid organization, you need to successfully deploy Client Access and Mailbox servers and configure your supported Voice over IP (VoIP) gateways to allow faxing. For details about how to deploy UM, see [Deploy Exchange 2013 UM](deploy-exchange-2013-um-exchange-2013-help.md). For details about how to deploy VoIP gateways and IP Private Branch eXchanges (PBXs), see [Connect UM to your telephone system in Exchange Server](connect-um-to-your-telephone-system-exchange-2013-help.md).

> [!IMPORTANT]
> Sending and receiving faxes using T.38 or G.711 isn't supported in an environment where Unified Messaging and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server are integrated.

### Step 2: Configure fax partner servers
<a name="step2configurefax"> </a>

Next, you need to enable incoming faxing and configure the fax partner's URI on each UM mailbox policy that you require in your organization. To successfully deploy incoming faxing, you must integrate a certified fax partner solution with Exchange Unified Messaging. For details, see [Fax advisor for Exchange UM](fax-advisor-for-exchange-um-exchange-2013-help.md).

> [!NOTE]
> Because the fax partner server is external to your organization, firewall ports must be configured to allow the T.38 protocol ports that enable faxing over an IP-based network. By default, the T.38 protocol uses TCP port 6004. It can also use User Datagram Protocol (UDP) port 6044, but this will be defined by the hardware manufacturer. The firewall ports must be configured to allow fax data that uses the TCP or UDP ports or port ranges defined by the manufacturer.

### Step 3: Enable faxing on Unified Messaging
<a name="step3configurefaxing"> </a>

Three components must be configured correctly for users to be able to receive faxes by using Unified Messaging:

- UM dial plans

- UM mailbox policies

- UM mailboxes

Faxing can be enabled or disabled on UM dial plans, UM mailbox policies, or on an individual UM-enabled user's mailbox. UM mailbox policies can be enabled or disabled for faxing using either the Exchange admin center (EAC) or the Exchange Management Shell. Enabling and disabling of dial plans and individual UM-enabled users needs to be done using the Exchange Management Shell. The following table shows the options that are available and the cmdlets and parameters that are used for enabling and disabling faxing.

|**UM component**|**Enable/disable using the EAC?**|**Shell example for enabling faxing**|
|:-----|:-----|:-----|
|Dial plan|No| `Set-UMDialPlan -id MyUMDialPlan -faxenabled $true`|
|UM mailbox policy|Yes| `Set-UMMaiboxPolicy -id MyPolicy -AllowFax $true`|
|UM-enabled user|No| `Set-UMMailbox -id tonysmith -faxenabled $true`|

By default, although the UM dial plan and the user's mailbox allow incoming faxes, you must first enable inbound faxing on the UM mailbox policy that's assigned to the UM-enabled user and then enter the fax partner server's URI.

To enable UM-enabled users to receive faxes, you must do the following:

- Verify that each UM dial plan allows the users who are associated with the dial plan to receive faxes. By default, all users who are associated with a dial plan can receive faxes. For UM-enabled users to receive fax messages in their mailbox, each VoIP gateway or IP PBX must be configured to accept incoming fax calls. You must also enable fax messages to be received by users who are linked with the dial plan. For more information about how to enable users linked with a dial plan to receive faxes or to prevent them from doing this, see [Enable a user to receive faxes](enable-a-user-to-receive-faxes-exchange-2013-help.md).

    > [!NOTE]
    > If you prevent fax messages from being received on a dial plan, no users who are associated with the dial plan will be able to receive faxes, even if you configure an individual user's properties to allow them to receive faxes. Enabling or disabling faxing on a UM dial plan takes precedence over the settings for an individual UM-enabled user.

- Configure the UM mailbox policy that's associated with the UM-enabled user. The UM mailbox policy must be configured to allow incoming faxes, including the fax partner's URI and the name of the fax partner's server. The _FaxServerURI_ parameter must use the following form: sip:\<_fax server URI_\>:\<_port_\>;\<_transport_\>, where "fax server URI" is either a fully qualified domain name (FQDN) or an IP address of the fax partner server. The "port" is the port on which the fax server listens for incoming fax calls and "transport" is the transport protocol that's used for the incoming fax (UDP, TCP, or Transport Layer Security (TLS)). For example, you might configure a UM mailbox policy to receive a fax as follows.

  ```powershell
  Set-UMMailboxPolicy MyUMMailboxPolicy -AllowFax $true -FaxServerURI "sip:faxserver.abc.com:5060;transport=tcp"
  ```

- For details, see [Set the partner fax server URI to allow faxing](set-the-partner-fax-server-uri-to-allow-faxing-exchange-2013-help.md).

    > [!CAUTION]
    > Although you can include multiple entries in the format for the _FaxServerURI_ by separating them with a semicolon, only one entry will be used. This parameter allows only one entry to be used, and adding multiple entries won't enable you to load balance fax requests.

- Verify that the mailbox that's UM-enabled can receive fax messages. By default, all users who are associated with a dial plan can receive faxes. However, there may be situations when a user can't receive faxes because the ability to receive faxes has been disabled on their mailbox. For more information about how to enable a UM-enabled user to receive faxes, see [Enable a user to receive faxes](enable-a-user-to-receive-faxes-exchange-2013-help.md).

    You can prevent an individual user who's associated with a dial plan from receiving fax messages. To do this, configure the properties for the user by using the **Set-UMMailbox** cmdlet in the Shell. You can also use the **Set-UMMailboxPolicy** cmdlet to prevent multiple users from receiving fax messages. For more information about how to prevent a user or users from receiving fax messages, see [Prevent a user from receiving faxes](prevent-a-user-from-receiving-faxes-exchange-2013-help.md).

### Step 4: Configure authentication
<a name="step4configureauthentication"> </a>

In addition to configuring your UM dial plans, UM mailbox policies, and UM-enabled users, you have to configure authentication between your Exchange servers and the fax partner server. The Exchange servers must be able to authenticate the origin of the messages that claim to be coming from the fax partner server. Any unauthenticated messages claiming to have come from a fax partner server won't be processed by an Exchange server.

To authenticate the connection from the fax partner server to the Exchange servers, you can use:

- Mutual TLS

- Sender ID validation

- A dedicated Receive connector

A Receive connector should be sufficient for authenticating the fax partner servers deployed in your organization. The Receive connector will ensure that the Exchange servers treats all traffic coming from the fax partner server as authenticated.

The Receive connector will be configured on an Exchange server that's used by the fax partner server to submit SMTP fax messages, and must be configured with the following values:

- _AuthMechanism: ExternalAuthoritative_

- _PermissionGroups: ExchangeServers, PartnersFax_

- _RemoteIPRanges: {the fax server's IP address}_

- _RequireTLS: False_

- _EnableAuthGSSAPI: False_

- _LiveCredentialEnabled: False_

For details, see [Receive connectors](receive-connectors-exchange-2013-help.md).

If the fax partner server sends network traffic to an Exchange server over a public network, for example, a service-based fax partner server hosted in the cloud, it's a good idea to authenticate the fax partner server using a sender ID check. This type of authentication ensures that the IP address that the fax message came from is authorized to send email messages on behalf of the fax partner domain that the message claims to have come from. DNS is used to store the sender ID records (or sender policy framework (SPF) records) and fax partners must publish their SPF records in the DNS forward lookup zone. Exchange will validate the IP addresses by querying DNS. However, the sender ID agent must be running on a Mailbox server to be able to perform the DNS query.

You can also use TLS to encrypt the network traffic, or mutual TLS for encryption and authentication between the fax partner server and Exchange servers.
