---
title: 'Protect voice mail: Exchange 2013 Help'
TOCTitle: Protect voice mail
ms:assetid: a88d41d5-2e70-4193-bcd3-dec50dff412b
ms:mtpsurl: https://technet.microsoft.com/library/Dd351041(v=EXCHG.150)
ms:contentKeyID: 49315484
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Protect voice mail in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Some legacy Private Branch eXchange (PBX) and IP PBX telephony systems allow the caller to mark a voice mail message as private, blocking the intended recipient of the message from forwarding it to others. In integrated voice mail systems, a voice message can be accessed in multiple ways, which makes it more of a challenge to prevent voice messages marked private from being exposed to unintended listeners.

Unified Messaging (UM) can be configured to use Active Directory Rights Management Services (AD RMS) to protect voice messages for an organization. This feature is known as Protected Voice Mail.

When a voice message is protected, the recipient is not only blocked from forwarding the message, but UM also ensures that only the intended recipient or recipients of the message can access its content. Protected voice messages can be accessed by using Microsoft Outlook 2010 or later, Outlook Web App, or Outlook Voice Access.

## Overview of Protected Voice Mail

The Protected Voice Mail feature is available with Exchange 2010 and later versions of Unified Messaging (UM). It can be configured on a UM mailbox policy, and all Protected Voice Mail settings can be configured by using the Exchange Management Console or the Shell in Exchange 2010 or by using the Exchange admin center (EAC) or cmdlets in the Shell in Exchange 2013.

Protected Voice Mail is implemented by applying Information Rights Management (IRM) to voice messages. When voice messages are protected by UM:

- Users can reply to protected voice messages.

- Recipients of a voice message can't forward it.

- Users can't save a copy of the voice message.

- Users can't save or copy the attached audio of the voice message.

- A voice message can be opened only by the intended recipient or recipients.

Both call-answering voice messages and interpersonal voice messages (voice messages that are sent to a user using Outlook Voice Access) can be protected by UM. However, protection won't be applied to the following types of messages:

- Fax messages.

- Non-voice messages. For example, email messages or meeting requests, even when they're created using Outlook Voice Access (voice replies).

## Overview of Active Directory Rights Management Services

AD RMS, a component of Windows Server 2008 and later versions, is available to help protect files so that only the users who the sender intends to view a file can do so. AD RMS protects a file by specifying the rights that a user must have to access the file. Rights can be configured to allow a user to open, modify, print, forward, or take other actions with the rights-managed information. With AD RMS, you can safeguard data when it's distributed outside your network.

An AD RMS system has both a server and a client component, including the following:

- A server that has Windows Server 2008 R2 or a later version installed that's running the Active Directory Rights Management Services server role, which handles certificates and licensing.

- A database server.

- The AD RMS client. The latest version of the AD RMS client is included as part of the Windows 7 and Windows 8 operating systems.

The server component is made up of several web services that run on a Microsoft server such as Windows Server 2008 or a later version. The client component can be run on either a client or server operating system and includes functions that enable an application to encrypt and decrypt content, retrieve templates and revocation lists, and acquire licenses and certificates from a server.

By using AD RMS and the AD RMS client, you can augment an organization's security strategy by protecting information through persistent usage policies that remain with the information, regardless of where it's moved. You can use AD RMS to help prevent sensitive information (such as financial reports, product specifications, customer data, and confidential email and voice mail messages) from intentionally or accidentally getting into the wrong hands. For detailed information, see [AD RMS Overview](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)).

In Exchange UM you can use Information Rights Management (IRM) features to apply persistent protection to messages and attachments.

Using the IRM features and Protected Voice Mail, your organization and your users can control the rights recipients have to access email and voice mail messages. IRM can be also used to restrict recipient actions such as forwarding a message to other recipients, printing a message or attachment, or extracting message or attachment content by copying and pasting.

## IRM requirements

Before you can implement IRM in Exchange, you must first deploy and configure your AD RMS infrastructure. For detailed information, see [Active Directory Rights Management Services](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)). To implement IRM to support Protected Voice Mail in your Exchange organization, your deployment must meet the following requirements.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Server</th>
<th>Requirement</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>AD RMS Cluster</p></td>
<td><ul>
<li><p>Windows Server 2008 R2 Standard or Enterprise with SP1 or Windows Server 2012 Standard or Datacenter. For more information about system requirements, see <a href="exchange-2013-system-requirements-exchange-2013-help.md">Exchange 2013 system requirements</a>.</p></li>
<li><p><strong>Service connection point (SCP)</strong>   Exchange 2013 and AD RMS-aware applications use the SCP registered in Active Directory to discover AD RMS clusters and URLs. AD RMS allows you to register the SCP within AD RMS setup. If the account used to set up AD RMS isn't a member of the Enterprise Admins security group, SCP registration can be performed after setup. There is only one SCP for AD RMS in an Active Directory forest.</p></li>
<li><p><strong>Permissions</strong>   Servers in the Exchange servers group or individual Exchange servers must be assigned Read and Execute permissions to the AD RMS server certification pipeline. The default path is \inetpub\wwwroot\_wmcs\certification\ServerCertification.asmx on AD RMS servers.</p></li>
<li><p><strong>AD RMS super users</strong>   To enable transport decryption, journal report decryption, IRM in Outlook Web App, and IRM for Exchange Search, you must add the Federated Delivery mailbox, a system mailbox created by Exchange Setup, to the AD RMS super users group on the AD RMS cluster. For detailed information, see <a href="add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md">Add the Federation Mailbox to the AD RMS Super Users Group</a>.</p></li>
</ul></td>
</tr>
</tbody>
</table>

## Configuring and testing IRM

You must use the Shell to configure IRM features. To configure individual IRM features, use the [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration) cmdlet. For more information about how to configure IRM features, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

After you've set up an Exchange server, you can use the [Test-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Test-IRMConfiguration) cmdlet to perform end-to-end tests of your IRM deployment. This cmdlet verifies the IRM configuration for an organization and should be run before enabling Protected Voice Mail. The **Test-IRMConfiguration** cmdlet performs the following tests:

- Inspects the IRM configuration for your Exchange organization.

- Checks the AD RMS server for version and hotfix information.

- Verifies whether an Exchange server can be activated for RMS by retrieving a Rights Account Certificate and Client Licensor Certificate (CLC).

- Acquires AD RMS rights policy templates from the AD RMS server.

- Verifies that the specified sender can send IRM-protected messages.

- Retrieves a super user use license for the specified recipient.

- Acquires a pre-license for the specified recipient.

## Client support and end-user features

The email client software that's used to listen to a Protected Voice Mail message must support IRM and know how to read a UM-protected voice message. Email clients that are supported include Microsoft Outlook 2010 or later versions, Outlook Web App, and Outlook Voice Access. The following table contains a list of email clients and whether they're supported.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Email client</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook</p></td>
<td><ul>
<li><p>Protected voice messages are supported in Outlook 2010 and later versions.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Outlook Web App</p></td>
<td><ul>
<li><p>Outlook Web App in Exchange 2010 or later versions supports Protected Voice Mail messages. Earlier versions of Outlook Web App, known as Outlook Web Access, don't support them.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Outlook Voice Access</p></td>
<td><ul>
<li><p>Outlook Voice Access in Exchange 2010 and later versions supports Protected Voice Mail. Outlook Voice Access included with Exchange 2007 doesn't support Protected Voice Mail.</p></li>
<li><p>The user's mailbox must reside on a Mailbox server in Exchange 2010 or a later version.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Exchange ActiveSync</p></td>
<td><ul>
<li><p>Protected Voice Mail is supported in Exchange 2010 SP1 and later versions.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Other email clients</p></td>
<td><ul>
<li><p>Protected Voice Mail isn't supported.</p></li>
</ul></td>
</tr>
</tbody>
</table>

## Protected voice message structure

There are actually two messages involved for each Protected Voice Mail message. The first message is the outer message, which isn't encrypted. It contains an attachment named message.rpmsg. The attachment contains the IRM-protected voice message and internal rights management control data. The rights management control data includes a content key and rights information that specifies who can access the voice message and how those users can access it.

Protected voice messages are shown in the user's Inbox in the **Voice Mail** search folder. The user can listen to the voice messages by using the embedded audio player just as they would listen to a regular voice message, except that the Forward button will be disabled and a note will be shown at the top of the message stating that it's protected and that it can't be forwarded.

For email clients that don't support Protected Voice Mail, the body of the outer message will be displayed. Administrators can include text when the client's software doesn't support Protected Voice Mail by using UM mailbox policies. You can customize the default text that's included in the email message by configuring a UM mailbox policy. For example, you could configure the UM mailbox policy with customized text such as, *"You can't open this voice mail message because it's protected. To view or listen to this voice message, sign in to your mailbox at https://mail.contoso.com or call +1 (425) 555-1234 to call in to Outlook Voice Access."*

## Composing a Protected Voice Mail message

There are two situations in which protected voice messages can be created:

- **Call answering**: Call answering occurs when a caller calls a UM-enabled user, but the user isn't available to answer the call or forwards it directly to voice mail. In call-answering scenarios, the voice mail system will play a series of voice prompts after the caller records a voice message.

  The caller can then choose from additional message options, including the option to mark the voice message as private by pressing the pound (\#) key. If the caller presses the \# key, they can follow the instructions provided by UM to mark the message as private, remove the private marking from the private voice message, or mark the voice message with High importance. The following diagram shows the menu options that are available to callers when they leave a private voice message for a user.

  > [!NOTE]
  > For call-answering calls, UM uses the Protected Voice Mail settings on the UM mailbox policy of the intended recipient of the message, because the caller isn't authenticated.

  **Create a Protected Voice Mail message using Call Answering**

  ![Create protected voice mail using call answering](images/Dd351041.4e9f50bf-5066-4d0a-b3eb-0515a2fc4560(EXCHG.150).jpg "Create protected voice mail using call answering")

- **Outlook Voice Access**: Outlook Voice Access lets UM-enabled users access their mailbox using analog, digital, or cellular telephones by dialing their Outlook Voice Access number. There are two Unified Messaging user interfaces available to UM-enabled users: the telephone user interface (TUI) and the voice user interface (VUI).

  Outlook Voice Access users can search for contacts in the directory and send them voice messages. If Protected Voice Mail has been enabled for the UM-enabled recipients, callers can mark the messages as private after they're recorded. Alternatively, administrators can configure a UM mailbox policy to ensure that all voice messages sent by authenticated users are protected by UM.

  > [!NOTE]
  > If a caller is authenticated, the Protected Voice Mail settings on the UM mailbox policy that's linked to the caller are applied, regardless of the UM mailbox policy settings for the intended recipient of the voice message.

  **Create a Protected Voice Mail message using the voice user interface**

  ![Create protected voice mail using voice interface](images/Dd351041.6b425ee4-5171-4a63-961f-bdbc6c79e1be(EXCHG.150).jpg "Create protected voice mail using voice interface")

  **Create a Protected Voice Mail message using the telephone user interface**

  ![Create protected voice mail using touchtone input](images/Dd351041.dd58fd38-c4c3-437c-adc1-497deb3c8a9f(EXCHG.150).jpg "Create protected voice mail using touchtone input")

## UM mailbox policies

You can create a Unified Messaging mailbox policy to apply a common set of UM policy settings, such as PIN policy settings, dialing restrictions, and Protected Voice Mail settings, to a collection of UM-enabled mailboxes. To learn more about UM mailbox policies, see [Manage a UM mailbox policy](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy).

You can use the EAC or the **Set-UMMailboxPolicy** cmdlet in the Shell to configure Protected Voice Mail options. The following table lists the settings that can be configured for Protected Voice Mail.

**Protected Voice Mail settings**

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Shell parameter</th>
<th>Setting available in EAC?</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>ProtectAuthenticatedVoiceMail</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>ProtectAuthenticatedVoiceMail</em> parameter specifies whether UM-enabled users can send protected voice messages when they're accessing their mailbox using Outlook Voice Access. The default setting is <code>None</code>. This means that no protection is applied when voice messages are composed and that callers won't have the option to mark voice messages as Private. If the value is set to <code>Private</code>, only messages marked as Private by the caller are protected. If the value is set to <code>All</code>, every voice message is protected, regardless of the option chosen by the caller.</p></td>
</tr>
<tr class="even">
<td><p><em>ProtectUnauthenticatedVoiceMail</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>ProtectUnauthenticatedVoiceMail</em> parameter specifies whether the Mailbox servers that answer calls for UM-enabled users associated with a UM mailbox policy create protected voice messages. This setting also applies when a message is sent from a UM auto attendant to a UM-enabled user. The default setting is <code>None</code>. This means that no protection is applied to voice messages and that the caller won't be offered the option to mark the message as Private. If the value is set to <code>Private</code>, only messages marked as Private by the caller are protected. If the value is set to <code>All</code>, every voice message is protected, regardless of whether if the message has been marked as private by the caller.</p></td>
</tr>
<tr class="odd">
<td><p><em>ProtectedVoiceMailText</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>ProtectedVoiceMailText</em> parameter specifies the text to be included in the body of the outer message of a Protected Voice Mail message. This text will be shown in all email client applications that don't support Protected Voice Mail messages. Note that a default message is always provided by UM when this property is set to <code>Null</code> or is empty.</p></td>
</tr>
<tr class="even">
<td><p><em>RequireProtectedPlayOnPhone</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>RequireProtectedPlayOnPhone</em> parameter specifies whether users associated with the UM mailbox policy will be forced to listen to the protected voice message over the phone (using Play On Phone). The default value is <code>$false.</code> When the value is set to <code>$true</code>, the audio media player on Protected Voice Mail forms in Outlook or Outlook Web App will be shown as disabled. Note that the preview text for the voice message can always be accessed. The user can't play the audio file using any media player software or use the embedded media player to listen to the voice message.</p></td>
</tr>
<tr class="odd">
<td><p><em>AllowVoiceResponseToOtherMessageTypes</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>AllowVoiceResponseToOtherMessageTypes</em> parameter specifies whether callers who have authenticated to Outlook Voice Access to access their email will be able to compose a voice reply to email messages and meeting requests.</p></td>
</tr>
</tbody>
</table>

For more information about how to manage Protected Voice Mail settings, see [Protected Voice Mail procedures](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/protected-voice-mail-procedures) or [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-UMMailboxPolicy).

## Text message notifications and Protected Voice Mail

Users who configure their UM account to send text message notifications (also called SMS notifications) to their mobile phone when voice messages are received will also receive audio transcription (Voice Mail Preview) text as part of the body of the text message. However, for protected voice messages, this represents a security issue because the content of the voice messages should always be protected.

When UM creates a text message notification for a voice message that's protected, it checks whether the voice message is marked as Private. If so, it won't add the transcribed audio text to the text message that it sends to the mobile phone. The following text will be included in the text message instead: "Use Outlook Voice Access to access this protected voice mail message."
