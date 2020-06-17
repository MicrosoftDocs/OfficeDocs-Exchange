---
title: 'POP3 and IMAP4 in Exchange Server 2013: Exchange 2013 Help'
TOCTitle: POP3 and IMAP4
ms:assetid: a7dc91ee-2919-4db3-ae9c-cd665d2e09ea
ms:mtpsurl: https://technet.microsoft.com/library/JJ657728(v=EXCHG.150)
ms:contentKeyID: 49300645
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# POP3 and IMAP4 in Exchange Server 2013

_**Applies to:** Exchange Server 2013_

Learn about the differences between POP3 and IMAP4 protocols in Exchange Server 2013 and how to configure options to access Exchange 2013 mailboxes from various email programs.

By default, POP3 and IMAP4 are disabled in Exchange Server 2013. To support POP3 clients that still rely on these protocols, you need to start two POP3 services: the Microsoft Exchange POP3 service and the Microsoft Exchange POP3 Backend service. To support IMAP4 clients that still rely on these protocols, you need to start two IMAP4 services: the Microsoft Exchange IMAP4 service and the Microsoft Exchange IMAP4 Backend service.

For detailed steps for enabling the POP3 and IMAP4 services, see [Enable POP3 in Exchange 2013](enable-pop3-in-exchange-2013-exchange-2013-help.md) and [Enable IMAP4 in Exchange 2013](enable-imap4-in-exchange-2013-exchange-2013-help.md).

By default, users who have mailboxes on computers that are running Exchange 2013 can access their mailboxes by using Outlook or Outlook Web App, Exchange ActiveSync, or Outlook Voice Access. Outlook, Outlook Web App, and Outlook Voice Access enable your email users to use the comprehensive set of features that are available to users who have mailboxes on Exchange 2016 servers.

## Overview of POP3 and IMAP4 functionality

This section describes the POP3 and IMAP4 functionality for Exchange 2013.

The POP3 and IMAP4 protocols have the following benefits and limitations:

- **POP3**: POP3 was designed to support offline mail processing. With POP3, email messages are removed from the server and stored on the local POP3 client unless the client has been set to leave mail on the server. This puts the data management and security responsibility in the hands of the user. POP3 doesn't offer advanced collaboration features such as calendaring, contacts, and tasks.

- **IMAP4**: IMAP4 offers offline and online access but, like POP3, IMAP4 doesn't offer advanced collaboration features such as calendaring, contacts, and tasks.

POP3 and IMAP4 email applications don't use POP3 and IMAP4 to send messages to the email server; they rely on the SMTP protocol to send messages. The connector for receiving email submissions from client applications that use POP3 or IMAP4 is created automatically when you install Exchange. For more information about connectors, see [Receive connectors](receive-connectors-exchange-2013-help.md).

> [!NOTE]
> Each time a user uses a POP or IMAP based email program to open their Microsoft 365 or Office 365 email, they may experience a delay of several seconds. The delay is caused by a proxy server, which introduces an additional hop for authentication. The proxy server first looks up the assigned pod server (Client Access server), and then authenticates against that.

## POP3 and IMAP4 cross-site connectivity

In earlier versions of Exchange, you had to perform a manual configuration step to allow your POP3 and IMAP4 clients to connect to their mail from one site in your organization when their mailbox was located in a different site in your organization. By default, Exchange 2013 automatically proxies from a Client Access server in one site to the correct server.

## Use non-standard accounts with POP3 and IMAP4

You can't use an Anonymous account or Guest account to sign in to an Exchange 2016 mailbox through POP3 or IMAP4. Anonymous accounts are blocked because of security vulnerabilities when you use non-standard accounts for POP3 and IMAP4 access. Additionally, you can't connect to the Administrator mailbox through POP3 or IMAP4. This limitation was included intentionally in Exchange 2013 to enhance security for the Administrator mailbox. To access the Administrator mailbox, you must use Outlook or Outlook Web App.

## Understand the differences between POP3 and IMAP4

**POP3**: POP3 is a frequently used email Internet protocol. By default, when POP3 email applications download email messages to a client computer, the downloaded messages are removed from the server. When a copy of your user's email isn't kept on the email server, the user can't access the same email messages from multiple computers. However, some POP3 email applications can be configured to keep copies of the messages on the server so that the same email messages can be accessed from another computer. POP3 client applications can only be used to download messages from the email server to a single folder (usually the Inbox) on the client computer. The POP3 protocol can't synchronize multiple folders on the email server with multiple folders on the client computer.

**IMAP4**: Email client applications that use IMAP4 are more flexible and generally offer more features than email client applications that use POP3. By default, when IMAP4 email applications download email messages to a client computer, a copy of downloaded messages remains on the email server. Because a copy of the user's email message is kept on the email server, the user can access the same email message from multiple computers. With IMAP4 email, the user can access and create multiple email folders on the email server. Users can then access any of their messages on the server from computers in multiple locations. For example, most IMAP4 applications can be configured to keep a copy of a user's sent items on the server so that they can view their sent items from any other computer. IMAP4 supports additional features that are supported by most IMAP4 applications. For example, some IMAP4 applications include a feature that lets the user view only the headers of their email messages on the server (who the message is from and the subject) and then download only the messages that they want to read. IMAP4 also doesn't support public folder access.

> [!NOTE]
> IMAP4 and POP3 clients have limited access to calendar information for Exchange. For more information, see <A href="configure-calendar-options-for-pop3-exchange-2013-help.md">Configure calendar options for POP3</A> and <A href="configure-calendar-options-for-imap4-exchange-2013-help.md">Configure calendar options for IMAP4</A>.

## Send and receive options for POP3 and IMAP4 email applications

POP3 and IMAP4 email applications let users choose when they want to connect to the server to send and receive email. This section discusses some of the most common connectivity options and also provides some factors your users should consider when they select connection options available in their POP3 and IMAP4 email applications.

## Common configuration settings

Three of the most common connection settings that can be set on the POP3 or IMAP4 client application are:

- Send and receive messages every time the email application is started. When this option is used, mail is only sent and received when the user starts the email application.

- Send and receive messages manually. When this option is used, messages are only sent and received when the user clicks a "send and receive" option in the client user interface.

- Send and receive messages every set number of minutes. When the user sets this option, the client application connects to the server every set number of minutes to send messages and download any new messages.

For information about how to configure these settings for the email application that you use, see the Help documentation that's provided with the respective email application.

## Considerations when setting send/receive options

If the device or computer that's running the POP3 or IMAP4 email application is always connected to the Internet, users may want to configure their email application to send and receive messages every set number of minutes. Connecting to the server at frequent intervals lets the user keep their email application up-to-date with the most current information on the server. However, if the device or computer that's running the POP3 or IMAP4 email application isn't always connected to the Internet, the user may want to configure the email application to send and receive messages manually.

> [!NOTE]
> If the user is using an IMAP4-compliant email application that supports the IMAP4 IDLE command, the user may be able to send email to and receive email from their Exchange mailbox in near real time. For this connection method to work, both the email server application and the client application must support the IMAP4 IDLE command. In most cases, users don't have to configure any settings in their IMAP4 application to use this connection method.

## POP3 and IMAP4 applications

Because Exchange 2013 supports POP3 and IMAP4, users can use any applications that support POP3 and IMAP4 client applications to connect to Exchange 2013. These applications include Outlook, Outlook Web App, Windows Live Mail, Outlook Express, Entourage, and many third-party applications such as Gmail, Mozilla Thunderbird and Eudora. The features supported by each email client applications vary. For information about the specific features offered by specific POP3 and IMAP4 client applications, see the documentation that's included with each application.

## User settings to configure POP3 or IMAP4 access to their Exchange 2013 mailboxes

After you enable POP3 and IMAP4 client access, you need to give users the information they'll need to connect their email programs to their Exchange 2016 mailbox.

To connect from inside the corporate network, users will need the following information:

- Internal POP3 or IMAP4 server name

- Internal POP3 or IMAP4 port number

- Internal POP3 or IMAP4 encryption method

- Internal SMTP (outgoing server) name

- Internal SMTP (outgoing server) port number

- Internal SMTP (outgoing server) encryption method

To connect from the Internet, they'll need the following information:

- External POP3 or IMAP4 server name

- External POP3 or IMAP4 port number

- External POP3 or IMAP4 encryption method

- External SMTP (outgoing server) name

- External SMTP (outgoing server) port number

- External SMTP (outgoing server) encryption method

You can make these settings available to your users through email or other manual communication methods. You can also configure Exchange so that your users can use Outlook Web App to look up their own settings.

**Configure Exchange so users can look up their POP3, IMAP4, and SMTP server settings**

By default, users can't look up their POP3, IMAP4, and SMTP server settings through Outlook Web App. To allow your users to do this, you need to use the **Set-PopSettings** and **Set-ImapSettings** cmdlets. To allow your users to look up their SMTP (outgoing) server settings, you must use the **Set-ReceiveConnector** cmdlet.

Do the following to allow users to look up their own POP3, IMAP4, and SMTP settings:

- **POP3 settings**: Run the **Set-POPSettings** cmdlet with the *ExternalConnectionSettings* parameter. Use the format `Set-POPSettings -ExternalConnectionSetting {<FQDN>:995:SSL}`. For example, run `Set-PopSettings -ExternalConnectionSetting {Dublin01.Contoso.com:995:SSL}` if you want clients that connect through the server with FQDN Dublin01.Contoso.com to be able to look up their own POP settings.

    You must restart IIS after applying this setting.

- **IMAP4 settings**: Run the **Set-IMAPSettings** cmdlet with the *ExternalConnectionSettings* parameter. Use the format `Set-ImapSettings -ExternalConnectionSetting {<FQDN>:993:SSL}`. For example, run `Set-IMAPSettings -ExternalConnectionSetting {Dublin01.Contoso.com:993:SSL}` if you want clients that connect through the server with FQDN Dublin01.Contoso.com to be able to look up their own IMAP setting.

    You must restart IIS after applying this setting.

- **SMTP settings**: Run the **Set-ReceiveConnector** cmdlet with the *AdvertiseClientSettings* parameter. Use the format `Set-ReceiveConnector "Client Frontend <Server Name>" -AdvertiseClientSettings $True -FQDN <FQDN>`. For example, run `Set-ReceiveConnector "Client Frontend <Server Name>" -AdvertiseClientSettings $True -FQDN Dublin01.Contoso.com` if you want clients that connect through the server with FQDN Dublin01.Contoso.com to be able to look up their own SMTP setting.

    You must restart IIS after applying this setting.

After you change your default settings by running the **Set-POPSettings**, **Set-IMAPSettings**, and **Set-ReceiveConnector** cmdlets, your users can look up their external POP, IMAP, and SMTP server settings in Outlook Web App by clicking **Settings** \> **Options** \> **Account** \> **My account** \> **Settings for POP or IMAP access**.

**Leave a copy of messages on the server**

The default setting on some email programs is to remove the messages on the server after they're retrieved. Be sure to recommend that your users set up their email program to keep a copy of all messages the client retrieves on the server. By keeping a copy of messages on the server, your users can access their messages from a different email program.
