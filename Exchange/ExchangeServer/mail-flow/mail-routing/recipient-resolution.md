---
localization_priority: Normal
description: 'Summary: Learn about recipient resolution in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 09deda5a-d405-45b1-a3ff-fefd3d76cdea
ms.reviewer:
title: Recipient resolution in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recipient resolution in Exchange Server

*Recipient resolution* is when the Exchange server expands and resolves all recipients in a message. Recipient resolution is responsible for these actions:

- Matching the recipient to the corresponding Active Directory object.

- Expanding distribution groups into a list of individual recipients.

- Applying message limits and any alternative recipients to each recipient.

Recipient resolution in Exchange 2016 or Exchange 2019 is basically unchanged from Exchange 2013. Recipient resolution is done by the categorizer in the Transport service on Mailbox servers. Each message is processed by the categorizer after the message is put in the Submission queue, but before the message is put in a delivery queue. The component of the categorizer that's responsible for recipient resolution is frequently called the *resolver*. For more information about the categorizer and the Submission queue, see [Understanding the Transport service on Mailbox servers](../mail-flow.md#understanding-the-transport-service-on-mailbox-servers).

This topic explains the various stages and components of recipient resolution that occur on the Exchange server.

## Top-level resolution

 *Top-level resolution* is the first stage of recipient resolution that associates each recipient in an incoming message to a matching recipient object in Active Directory. The categorizer creates a list that contains the sender and the initial, unexpanded recipient email addresses from the message, and uses that list to query Active Directory. When a match is found in the email address properties for mail-enabled Active Directory objects, the categorizer caches the properties of the objects, and enforces any sender message restrictions.

### Recipient email addresses

Top-level resolution begins with a message and the initial, unexpanded list of recipients from the *message envelope*. The message envelope contains the SMTP commands that are used to transmit messages between messaging servers:

- The sender's email address is contained in the **MAIL FROM** command.

- Each recipient's email address is contained in a separate **RCPT TO** command.

The envelope sender and envelope recipients are typically created from the sender and recipients in the `To:`, `From:`, `Cc:`, and `Bcc:` header fields in the message header. However, these header fields are easily forged and may not match the actual sender or recipient email addresses that were used to transmit the message.

#### Encapsulated email addresses

Standard SMTP email addresses follow the specifications in RFC 5321 and RFC 5322 (for example chris@contoso.com). However, an email address can also be a non-SMTP address that's encapsulated in an SMTP address. Exchange supports the Internet Mail Connector Encapsulated Address (IMCEA) encapsulation method that replaces characters that would be invalid in an SMTP email address with valid characters.

IMCEA addresses use the syntax `IMCEA<Type>-<address>@<domain>`:

- _\<Type\>_ identifies the type of non-SMTP address (for example `EX`, `X400`, or `FAX`). Although `SMTP` and `X500` are theoretically valid values for \<_Type_\>, Exchange recipient resolution rejects any IMCEA-encoded addresses that use either of these types.

- _\<address\>_ is the encoded version of original address:

  - Alphanumeric characters, the equal sign (=) and the hyphen (-) aren't replaced.

  - Forward slashes (/) are replaced by underscores (_).

  - Other US-ASCII characters are replaced by a plus sign (+) and the two digits of the ASCII value in hexadecimal (for example, the space character has the encoded value `+20`).

- _\<domain\>_ is SMTP domain that's used to encapsulate the non-SMTP address (for example, contoso.com).

IMCEA addresses are returned to their original values (unencapsulated) only when the domain matches the default accepted domain in the Exchange organization. For more information about the default accepted domain, see [Default domain](../accepted-domains/accepted-domains.md#default-domain).

The maximum length for an SMTP email address in Exchange is 571 characters. This limit includes:

- 315 characters for the name part of the address

- 255 characters for the domain name

- The at sign (@) character that separates the name from the domain name

Exchange doesn't support IMCEA-encoded messages where the name part of the address exceeds 315 characters, even if the complete email address is less than 571 characters.

### Address resolution

For each message, the sender's email address and all recipient addresses are added to a list that's used to query Active Directory. Any encapsulated addresses are unencapsulated before they're added to the list. The Active Directory query is performed on up to 20 email addresses at a time. If the query encounters transient errors, the message is returned to the Submission queue and deferred for the time that's specified by the _ResolverRetryInterval_ key in the `%ExchangeInstallPath%Bin\EdgeTransport.exe.config` XML application configuration file that's associated with the Transport service. The default value is 30 minutes.

The Active Directory recipient objects that are used by Exchange are described in the following table. For more information about Exchange recipient types, see [Recipients](../../recipients/recipients.md).

|**Active Directory recipient type**|**Description**|
|:-----|:-----|
|DistributionGroup|Any mail-enabled group object. The distribution group object types are: <br/>• **MailUniversalDistributionGroup**: A universal distribution group object <br/>• **MailUniversalSecurityGroup**: A universal security group (USG) object that has an email address|
|DynamicDistributionGroup|An object that has the Active Directory class **msExchDynamicDistributionList**. For more information, see [Manage dynamic distribution groups](../../recipients/dynamic-distribution-groups/dynamic-distribution-groups.md).|
|Mailbox|An object that has an email address and a defined _Database_ parameter.|
|MailUser|A user object that has an email address without a defined _Database_ parameter. For more information, see [Manage mail users](../../recipients/mail-users.md).|
|MailContact|A contact object that has an email address. Typically, a mail contact is used for recipients outside the Exchange organization. A mail contact is also used in cross-forest Exchange environments. For more information, see [Manage mail contacts](../../recipients/mail-contacts.md).|
|MailPublicFolder|A public folder object that has an email address. For more information, see [Public folders](../../collaboration/public-folders/public-folders.md).|
|MicrosoftExchangeRecipient|An object that has the Active Directory class **msExchExchangeServerRecipient**. For more information about the Exchange recipient object, see [Recipients](../../recipients/recipients.md).|
|SystemMailbox|A user object that has an email address and is located in the Microsoft Exchange System Objects container. There should be one system mailbox for each mailbox database in the Exchange organization.|

The Active Directory query classifies an object with missing or malformed critical properties as an invalid object (for example, a dynamic distribution group object without an email address). Messages sent to recipients that are classified as invalid objects generate a non-delivery report (also known as an NDR or bounce message).

For each email address, the categorizer does a single initial query for all possible recipient properties (for example, the recipient identifiers, recipient type, message limits, email addresses, and alternative recipients). The applicable recipient properties are cached for later use. Recipient resolution classifies recipients based on similarities in how the recipients are resolved, and the similarity of the applicable recipient properties.

The LDAP filter that's used for address resolution depends on the recipient's email address:

- For the **EX** email address type, the LDAP filter is based on the recipient's **legacyExchangeDN** attribute (higher priority) or the recipient's **proxyAddresses** attribute (lower priority).

- For all other email addresses types, the recipient **proxyAddresses** attribute is used as the LDAP filter.

If the email address doesn't match the recipient's primary SMTP address, the categorizer rewrites the email address in the message to match the primary SMTP address. The original email address is saved in the `ORCPT=` entry in the **RCPT TO** command in the message envelope.

### Sender message restrictions

The size of a message can change because of content conversion, encoding, and agent processing. When a message enters the Exchange organization, the original size of the message is recorded in the **X-MS-Exchange-Organization-OriginalSize:** header field in the message header. The lower value of the current message size or the original message size is used to enforce sender message size limits. If the original message size header field doesn't exist, it's created using the current size of the message. If the message is too large, it's returned to the sender in an NDR, and additional message processing is stopped.

The sender recipient limit is only enforced in the Transport service on the first Mailbox server that processes the message. The original, unexpanded message envelope recipient count is compared to the sender recipient limit.

The message sender and all recipients are marked as resolved by stamping an extended property in the message. This extended property allows the message to bypass top-level resolution if the message goes through recipient resolution again (for example, because the Exchange Transport service restarted.

## Expansion

Expansion occurs after top-level resolution. Expansion completely expands nested levels of recipients into individual recipients. Expansion may require multiple trips through the expansion process to expand all recipients. Not all recipients have to be expanded. However, all recipients go through the expansion process to enforce recipient message restrictions for all kinds of recipients.

The types of recipients that require expansion are described in this list:

- **Distribution groups and dynamic distribution groups**: Distribution groups are expanded based on the **memberOf** Active Directory property. Dynamic distribution groups are expanded by using the Active Directory query definition. If the _ExpansionServer_ parameter is set on the group in the Exchange Management Shell, the group is routed to the specified server for expansion.

  **Note:** When you specify an expansion server for a group, the group becomes dependent on the availability of the expansion server (messages can't be delivered to the group if the expansion server is unavailable). Therefore, consider implementing high availability solutions for expansion servers.

- **Alternative recipients**: You can configure mailboxes and mail-enabled public folders to forward messages to other recipients:

  - **Mailboxes**: You can configure forwarding to another recipient in the Exchange organization, or to an external email address. For more information, see [Configure email forwarding for a mailbox](../../recipients/user-mailboxes/email-forwarding.md).

  - **Mail-enabled public folders**: You can configure forwarding to another recipient in the Exchange organization.

    You can configure the mailbox or mail-enabled public folder to only send messages to the forwarding address, or to the forwarding address and the original recipient.

- **Contact chains**: A *contact chain* is a mail user or mail contact where the external email address is set to the email address of another recipient in the Exchange organization.

### Recipient loop detection

As groups, alternative recipients, and contacts chains are expanded, the categorizer checks for *recipient loops*. A recipient loop is a configuration problem that causes message delivery to the same recipients in an endless circle. The different types of recipient loops are described in this list:

- **Harmless recipient loop**: These are the two scenarios when harmless recipient can loops occur:

  - When two groups contain one another as members.

  - When mailboxes or mail-enabled public folders are set to deliver and forward to one another (the message is delivered to the original recipient and forwarded).

    When the categorizer detects a harmless recipient loop, the message is delivered to the recipient, but no additional attempts are made to deliver the message to the same recipient.

- **Broken recipient loop**: When mailboxes or mail-enabled public folders are set to forward to one another (the messages are only forwarded).

  A broken recipient loop can't result in successful message delivery. When the categorizer detects a broken recipient loop, expansion activity for the current recipient stops, and an NDR is generated.

Recipient loop detection doesn't prevent duplicate message delivery. For example, consider this scenario:

- Distribution Group A has Distribution Group B and Distribution Group C as members.

- Distribution Group C is also a member of Distribution Group B.

In this scenario, Distribution Group C will experience duplicate message delivery.

### Delivery report redirection for groups

When a group is expanded, the message type is checked to see if it's a delivery report message. If the message is a delivery report, the redirection settings of the group are checked to see if redirection of the delivery report is required. You may want to suppress delivery reports for the group because a delivery report might disclose unwanted information about the membership of the group.

The delivery report redirection settings that are available in the Exchange Management Shell for distribution groups and dynamic distribution groups are described in this list:

- **ReportToManagerEnabled parameter**: Enables or disables sending delivery reports to the group manager. Valid values are `$true` or `$false`. The default value is `$false`. For a distribution group, the manager is controlled by the _ManagedBy_ parameter on the **Set-Group** (distribution groups), or **Set-DynamicDistributionGroup** (dynamic distribution groups) cmdlets.

- **ReportToOriginatorEnabled parameter**: Enables or disables sending delivery reports to the message sender for messages that are sent to the group. Valid values are `$true` or `$false`. The default value is `$true`.

  **Note**: The values of _ReportToManagerEnabled_ parameter and _ReportToOriginatorEnabled_ can't both be `$true`. If one parameter is set to `$true`, the other must be set to `$false`. The values of both parameters can be `$false`, which suppresses the redirection of all delivery report messages for the group.

The different types of delivery report messages that can be affected by delivery report redirection for groups are described in this list:

- **Delivery receipt (DR)**: Confirms that a message was delivered to its intended recipient.

- **Delivery status notification (DSN)**: Describes the result of an attempt to deliver a message that didn't result in the message being returned to the sender in an non-delivery report (NDR). For more information about DSN messages, see [DSNs and NDRs in Exchange Server](../../mail-flow/non-delivery-reports-and-bounce-messages/non-delivery-reports-and-bounce-messages.md).

- **Message disposition notification (MDN)**: Describes the status of a message after it has been successfully delivered to a recipient. Read notifications (RNs) and non-read notification (NRNs) are both examples of MDN messages. MDN messages are defined in RFC 2298 and are controlled by the **Disposition-Notification-To:** header field in the message header. MDN settings that use this header field are compatible with many different kinds of messaging servers. MDN settings can also be defined by using MAPI properties in Outlook and Exchange.

- **Non-delivery report (NDR)**: Indicates to the message sender that the message couldn't be delivered to the specified recipients. The message is returned to the sender in the NDR.

- **Non-read notification (NRN)**: Indicates that a message was deleted before it was read.

- **Out of office (OOF)**: Indicates that the recipient won't respond to email messages. The acronym OOF dates back to the original Microsoft messaging system where the corresponding notification was named "out of facility."

- **Read notification (RN)**: Indicates that a message was read.

- **Recall Report**: Indicates the status of a recall request for a specific recipient (the sender tried to recall a sent message by using Outlook).

These are the settings that cause delivery report messages to be deleted when they're sent to a group:

- Report redirection isn't configured for the group, or report redirection is set to the message sender.

- Report redirection is set to the group manager, and the delivery report message isn't an NDR.

If report redirection is set to the group manager, and the delivery report message is an NDR, the message is delivered to the group manager.

The affect of group delivery report redirection settings on regular messages that contain report requests are described in this list:

- If report redirection is set to the message sender, the report request settings aren't modified.

- If report redirection isn't configured for the group, all report requests are suppressed. The `NOTIFY=NEVER` entry is added to **RCPT TO** for each recipient in the message envelope.

- If report redirection is set to the group manager, NDRs are sent to the group manager, but all other report requests are suppressed.

### Message restrictions on recipients

The expansion process also enforces any message restrictions that are configured on recipients. These restrictions may be configured individually for each recipient or organizationally for all servers in the Exchange organization.

For more information on message size limits, see [Recipient limits](../message-size-limits.md#recipient-limits) and [Organizational limits](../message-size-limits.md#organizational-limits).

|**Restriction**|**EAC configuration**|**Exchange Management Shell configuration**|**Description**|
|:-----|:-----|:-----|:-----|
|Maximum size of a message received|Organization: **Mail flow** \> **Receive connectors** \> **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png) \> **Organization transport settings** \> **Limits** tab \> **Maximum receive message size (MB)** <br/> Mailboxes: **Recipients** \> **Mailboxes** \> select the mailbox \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mailbox features** \> **Mail flow** section \> **Message size restrictions** section \> **View details** \> **Received messages** section \> **Maximum message size (KB)** <br/> Mail users: **Recipients** \> **Contacts** \> select the mail user \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mail flow settings** \> **Message size restrictions** section \> **View details** \> **Received messages** section \> **Maximum message size (KB)**|Organization cmdlet: **Set-TransportConfig** <br/> Recipient cmdlets: **Set-DistributionGroup**, **Set-DynamicDistributionGroup**, **Set-Mailbox**, **Set-MailContact**, **Set-MailPublicFolder**, and **Set-MailUser** <br/> Parameter: _MaxReceiveSize_|Specifies the maximum size of a message, which includes the message header, the message body, and any attachments. Whenever Exchange checks the message size, the lower value of the current message size or the original message size (the **X-MS-Exchange-Organization-OriginalSize:** message header) is used. The size of the message can change because of content conversion, encoding, and transport agent processing. <br/> **Note**: The specified maximum message size is inflated by approximately 33% to account for Base64 encoding (for example, the specified value 64 MB results in a realistic maximum message size of approximately 48 MB). <br/> For more information, see [Message size and recipient limits in Exchange Server](../../mail-flow/message-size-limits.md).|
|The recipient can only accept messages from internal senders, and must reject messages from external senders|Mailboxes: **Recipients** \> **Mailboxes** \> select the mailbox \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mailbox features** \> **Mail flow** section \> **Message delivery restrictions** section \> **View details** \> **Accept messages from** section \> check or uncheck **Require that all senders are authenticated** <br/> Remote mailboxes: **Recipients** \> **Mialboxes** \> select the Office 365 mailbox \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mail flow settings** \> **Message delivery restrictions** section \> **View details** \> **Accept messages from** section \> check or uncheck **Require that all senders are authenticated** <br/> Mail users: **Recipients** \> **Contacts** \> select the mail user \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mail flow settings** \> **Message delivery restrictions** section \> **View details** \> **Accept messages from** section \> check or uncheck **Require that all senders are authenticated** <br/> Groups: **Recipients** \> **Groups** \> select the group \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Delivery management** \> select **Only senders inside my organization** or **Senders inside and outside of my organization**|Cmdlets: **New-DistributionGroup**, **Set-DistributionGroup**, **Set-DynamicDistributionGroup**, **Set-Mailbox**, **Set-MailContact**, **Set-MailPublicFolder**, **Set-MailUser**, and **Set-RemoteMailbox** <br/> Parameter: _RequireSenderAuthenticationEnabled_|You can configure the recipient to only accept messages from authenticated (internal) senders, or to accept messages from authenticated and unauthenticated (external) senders.|
|Senders who are allowed or aren't allowed to send messages to the recipient|Mailboxes: **Recipients** \> **Mailboxes** \> select the mailbox \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mailbox features** \> **Mail flow** section \> **Message delivery restrictions** section \> **View details** \> **Accept messages from** section: **All senders** or **Only senders in the following list** or **Reject messages from** section: **No senders** or **Senders in the following list** <br/> Remote mailboxes: **Recipients** \> **Mialboxes** \> select the Office 365 mailbox \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mail flow settings** \> **Message delivery restrictions** section \> **View details** \> **Accept messages from** section: **All senders** or **Only senders in the following list** or **Reject messages from** section: **No senders** or **Senders in the following list** <br/> Mail users: **Recipients** \> **Contacts** \> select the mail user \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mail flow settings** \> **Message delivery restrictions** section \> **View details** \> **Accept messages from** section: **All senders** or **Only senders in the following list** or **Reject messages from** section: **No senders** or **Senders in the following list** <br/> Groups: **Recipients** \> **Groups** \> select the group \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Delivery management** \> click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png) or **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png) to specify users or group members who can send to the group (messages from others senders are rejected).|Cmdlets: **Set-DistributionGroup**, **Set-DynamicDistributionGroup**, **Set-Mailbox**, **Set-MailContact**, **Set-MailPublicFolder**, **Set-MailUser**, and **Set-RemoteMailbox** <br/> Accept parameters: _AcceptMessagesOnlyFromSendersOrMembers_ (or _AcceptMessagesOnlyFrom_ for individual recipients only and _AcceptMessagesOnlyFromDLMembers_ for group members only) <br/> Reject parameters: _RejectMessagesFromSendersOrMembers_ (or _RejectMessagesOnlyFrom_ for individual recipients only and _RejectMessagesOnlyFromDLMembers_ for group members only)|The categorizer checks the recipient permission in two passes. The first pass determines whether the sender is present in the accept or reject lists. If the sender isn't found in either list, the distribution groups in those parameters are fully expanded. This complete group expansion might take some time, so we recommend that you minimize the depth of nested groups in the accept or reject lists.|

Certain types of messages that are sent by authenticated senders are exempt from restrictions. The following list describes the messages that are exempt from recipient restrictions:

- **Messages sent by the Microsoft Exchange recipient**: These messages include DSNs and NDRs , journal reports, quota messages, and other system-generated messages that are sent to internal message senders. For more information about the Microsoft Exchange recipient, see [Recipients](../../recipients/recipients.md).

- **Messages sent by the external postmaster address**: These messages include DSNs and NDRs, and other system-generated messages that are sent to external message senders. For more information about the external postmaster address, see [Managing the External Postmaster Address](https://docs.microsoft.com/exchange/configure-the-external-postmaster-address-exchange-2013-help).

Exchange blocks certain types of messages that are sent to external domains (for example, internal OOF messages, automatic replies, and meeting forward notifications). You configure these settings in remote domains (the default remote domain, or remote domains for specific external domains). For more information, see [Managing Remote Domains](https://docs.microsoft.com/exchange/remote-domains-exchange-2013-help).

## Bifurcation and controlling recipient expansion

Because the complete list of message recipients is expanded and resolved by recipient resolution, there are occasions when different copies of the same message need to be created:

- **Recipients require different message settings**: Creating a new version of the message that has slightly different properties than the original is called *bifurcation*. For example, Exchange might need to bifurcate a message when read receipts are enabled for some recipients and blocked for others.

- **Limit the number of envelope recipients in a single message**: Expanding large group can generate thousands of individual recipients. Instead of creating a single copy of the message that has thousands of envelope recipients, Exchange creates multiple copies of the same message that have a limited number of recipients in the message envelope.

### Bifurcation

Recipient resolution bifurcates a message if the following conditions are true:

- When the message sender in **MAIL FROM** in the message envelope is updated (for example, when the _ReportToManagerEnabled_ parameter on a group has the value `$true`).

- When auto-response messages (for example, DSNs and NDRs, OOF messages, and recall reports) need to be suppressed.

- When alternative recipients are expanded.

- When a **Resent-From:** header field is added to the message header. Resent header fields are informational header fields that can be used to determine whether a message has been forwarded by a user. Resent header fields are used so that the message appears to the recipient as if it was sent directly by the original sender. The recipient can view the message header to discover who forwarded the message. Resent header fields are defined in section 3.6.6 of RFC 5322.

- When the expansion history of the group needs to be transmitted.

### Controlling recipient expansion

When the number of expanded recipients is too large, the categorizer splits the message into multiple copies to reduce the system resources that are used during message expansion. The maximum number of envelope recipients in a message is controlled by the _ExpansionSizeLimit_ key in the `%ExchangeInstallPath%Bin\EdgeTransport.exe.config` application configuration file. The default value is 1000.

> [!CAUTION]
> We recommend that you don't modify the value of the _ExpansionSizeLimit_ key on an Exchange transport server in a production environment.

## Recipient resolution diagnostics

Exchange provides reporting and diagnostic information for recipient resolution in performance counters, message tracking log entries, and recipient resolution logging. These sources can help you identify and diagnose problems with recipient resolution.

### Recipient resolution performance counters

The performance counters that are available for recipient resolution are described in this table.

|**Counter name**|**Display name**|**Description**|
|:-----|:-----|:-----|
|AmbiguousRecipientsTotal|Ambiguous Recipients|The total number of ambiguous recipients that were detected during recipient resolution. Ambiguous recipients are different recipients that have matching **legacyExchangeDN** Active Directory attributes or matching **proxyAddresses** Active Directory attributes.|
|AmbiguousSendersTotal|Ambiguous Senders|This is the number of ambiguous senders that were detected during recipient resolution. Ambiguous senders are different senders that have matching **legacyExchangeDN** Active Directory attributes or matching **proxyAddresses** Active Directory attributes.|
|FailedRecipientsTotal|Failed Recipients|The number of failed recipients that were detected during recipient resolution.|
|LoopRecipientsTotal|Loop Recipients|The number of recipients that failed recipient resolution because of recipient loops.|
|MessagesChippedTotal|Messages Chipped|The total number of copies of the same message that were created during recipient resolution to control the number of envelope recipients in a single message. This process is referred to as *chipping*.|
|MessagesCreatedTotal|Messages Created|The number of messages that were created during recipient resolution.|
|MessagesRetriedTotal|Messages Retried|The number of messages that were scheduled for retry during recipient resolution.|
|UnresolvedOrgRecipientsTotal|Unresolved Org Recipients|The number of unresolved recipients from an authoritative domain that were detected during recipient resolution.|
|UnresolvedOrgSendersTotal|Unresolved Org Senders|The number of unresolved senders from an authoritative domain that were detected during recipient resolution.|

### Recipient resolution events in the message tracking log

The recipient resolution events that are written in the message tracking log are described in this table.

|**Message tracking event**|**Description**|
|:-----|:-----|
|**EXPAND**|A distribution group was expanded.|
|**REDIRECT**|A message was redirected to the forwarding address that's configured on the mailbox or mail-enabled public folder.|
|**RESOLVE**|A recipient's email address was changed to the primary SMTP email address of the corresponding Active Directory recipient object (in other words, the message was sent to a proxy address of the recipient).|
|**TRANSFER**|Message bifurcation or chipping occurred (for example, due to content conversion, message recipient limits, or transport agents).|

For more information about message tracking, see [Message tracking](../../mail-flow/transport-logs/message-tracking.md).

### Recipient resolution logging

Recipient resolution logging is controlled by the _ResolverLogLevel_ key in the `%ExchangeInstallPath%Bin\EdgeTransport.exe.config` application configuration file. Valid values for this key are:

- `Disabled`: No recipient resolution data is logged. This is the default value.

- `Enabled`: Only message envelope data is logged.

- `FullContent`: Message envelope data and message header data is logged

The log files are stored at `%ExchangeInstallPath%Logging\Resolver`.

> [!NOTE]
> Any customized Exchange or Internet Information Server (IIS) settings that you made in Exchange XML application configuration files on the Exchange server (for example, web.config files or the EdgeTransport.exe.config file) **will be overwritten** when you install an Exchange CU. Be sure save this information so you can easily re-apply the settings after the install. After you install the Exchange CU, you need to re-configure these settings.
