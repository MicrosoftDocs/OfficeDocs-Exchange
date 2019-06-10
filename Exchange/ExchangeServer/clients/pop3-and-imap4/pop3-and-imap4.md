---
localization_priority: Normal
description: 'Summary: Learn about support for POP3 and IMAP4 in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: a7dc91ee-2919-4db3-ae9c-cd665d2e09ea
ms.date: 7/5/2018
ms.reviewer: 
title: POP3 and IMAP4 in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# POP3 and IMAP4 in Exchange Server

Although users typically access their Exchange mailboxes by using Outlook (MAPI), Outlook on the web (formerly known as Outlook Web App), and Exchange ActiveSync, POP3 and IMAP4 are available in Exchange Server 2016 and Exchange Server 2019. To support clients that still rely on these protocols, you need to start the services, and configure the settings for POP3 and IMAP4. For detailed instructions, see the following topics:

- [Enable and configure POP3 on an Exchange server](configure-pop3.md)

- [Enable and configure IMAP4 on an Exchange server](configure-imap4.md)

- [Configure authenticated SMTP settings for POP3 and IMAP4 clients in Exchange Server](configure-authenticated-smtp.md)

After you enable and configure POP3 or IMAP4 on the Exchange server, you can enable or disable POP3 or IMAP4 access to specific mailboxes. For more information, see [Enable or disable POP3 or IMAP4 access to mailboxes in Exchange Server](configure-mailbox-access.md).

 **Note**: Clients connect to the POP3 and IMAP4 services in the Client Access (frontend) services on the Mailbox server. They never connect directly to the POP3 and IMAP4 backend services. For more information, see [Client access protocol architecture](../../architecture/architecture.md#ClientAccessProtocol).

## POP3 and IMAP4 improvements in Exchange Server

POP3 and IMAP4 functionality in Exchange 2016 and Exchange 2019 is basically unchanged from Exchange 2013. These are the improvements in POP3 and IMAP4 as compared to Exchange 2010:

- By default, the Client Access (frontend) services in Exchange 2016 and 2019 automatically proxy POP3 and IMAP4 client connections from one Active Directory site to the correct Mailbox server in a different Active Directory site. In previous versions of Exchange, you had to perform a manual configuration step to allow POP3 and IMAP4 clients to connect to their mailboxes from one site to another.

- You can't use the Anonymous or Guest accounts to access an Exchange 2016 or Exchange 2019 mailbox by using POP3 or IMAP4. Access is blocked to prevent security vulnerabilities when you use non-standard accounts for POP3 and IMAP4 access.

- You can't connect to the Administrator mailbox by using POP3 or IMAP4 (you can use Outlook or Outlook Web App). This limitation was intentionally included in Exchange 2016 to enhance security for the Administrator mailbox.

## Overview of POP3 and IMAP4 functionality
<a name="Overview"> </a>

The POP3 and IMAP4 protocols have the following benefits and limitations:

- **POP3**

  - Designed for offline message processing.

  - Can only download messages from a single folder (usually the Inbox) in the mailbox to a single folder in the POP3 application on the client computer or device.

  - By default, downloaded messages are removed from the email server, and are stored only on the local computer or device. Therefore, users can't access the same email messages from multiple computers or devices (although many POP3 applications can be configured to keep copies of downloaded messages in the mailbox on the email server).

  - Doesn't offer advanced collaboration features such as calendaring, contacts, and tasks.

- **IMAP4**

  - Offers offline and online message processing.

  - Can synchronize messages from multiple folders in the mailbox with the client computer or device. For example, most IMAP4 applications can be configured to keep a copy of sent messages in the mailbox on the email server.

  - By default, copies of downloaded messages remain on the email server. Therefore, users can access the same messages from multiple computers.

  - Supports additional features. For example, you can download the message headers (the message's sender and subject) before you decide to download the complete message.

  - Doesn't offer advanced collaboration features such as calendaring, contacts, and tasks.

 **Note**: POP3 and IMAP4 clients have limited access to Exchange calendar information. For more information, see [Configure Calendar Options for POP3](http://technet.microsoft.com/library/ac3d60a0-8697-4c06-9e93-f8d2c4b157b6.aspx) and [Configure Calendar Options for IMAP4](http://technet.microsoft.com/library/6679c8b2-3f0f-449a-a17c-a7b30001538c.aspx).

## POP3 and IMAP4 applications and settings
<a name="SendReceive"> </a>

After you've enabled and configured the required services, users can connect to their Exchange mailboxes by using any application that support POP3 and IMAP4. For example, Outlook, Windows Mail, and Mozilla Thunderbird. POP3 and IMAP4 feature support varies by application, so check the application's documentation.

Verify the POP3 or IMAP4 email program is configured to keep a copy of all messages on the server. This allows the users to access their messages from different computers or applications.

Another important setting is how frequently the email program contacts the server to send and receive mail. There are three basic settings:

- **Send and receive messages when the application is started**

- **Send and receive messages manually**: Messages are only sent and received when the user clicks a "send and receive" option in the application. This is a good setting for computers that aren't always connected to the Internet (for example, dial-up or metered Internet connections).

- **Send and receive messages every set number of minutes**: The application connects to the email server periodically to send messages and to download any new messages. This is a good setting for computers that are always connected to the Internet, because the application is kept up-to-date with the most current messages from the mailbox.

 **Note**: If the application and server both support the IMAP4 **IDLE** command, users can send and receive messages in near real time (Exchange supports the IMAP4 **IDLE** command). In most cases, users don't need to configure any settings in their IMAP4 application to use this connection method.

To configure a POP3 or IMAP4 client to connect to a mailbox, users need specific information about the POP3 or IMAP4 settings. By default, Exchange uses the following settings for **internal** POP3 connections:

- **POP3 server FQDN**: `<ServerFQDN>`. For example, `mailbox01.contoso.com`.

- **TCP port and encryption method**: 995 for always SSL/TLS encrypted connections, and 110 for unencrypted connections, or for opportunistic TLS (**STARTTLS**) that results in an encrypted connection after the initial plain text protocol handshake.

To allow **external** POP3 clients to connect to mailboxes, you need to configure these settings for external connections. For more information, see [Enable and configure POP3 on an Exchange server](configure-pop3.md).

By default, Exchange uses the following settings for **internal** IMAP4 connections:

- **IMAP4 server FQDN**: `<ServerFQDN>`. For example, `mailbox01.contoso.com`.

- **TCP port and encryption method**: 993 for always SSL/TLS encrypted connections, and 143 for unencrypted connections, or for opportunistic TLS (**STARTTLS**) that results in an encrypted connection after the initial plain text protocol handshake.

To allow **external** IMAP4 clients to connect to mailboxes, you need to configure these settings for external connections. For more information, see [Enable and configure IMAP4 on an Exchange server](configure-imap4.md).
