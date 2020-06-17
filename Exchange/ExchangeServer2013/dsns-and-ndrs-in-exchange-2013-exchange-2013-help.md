---
title: 'DSNs and NDRs in Exchange 2013: Exchange 2013 Help'
TOCTitle: DSNs and NDRs in Exchange 2013
ms:assetid: 8e91de84-76fa-49b2-898c-c5eface76560
ms:mtpsurl: https://technet.microsoft.com/library/Bb232118(v=EXCHG.150)
ms:contentKeyID: 49286851
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# DSNs and NDRs in Exchange 2013

_**Applies to:** Exchange Server 2013_

> [!NOTE]
> If you need help with NDRs in Office 365 or Exchange Online, see <A href="https://docs.microsoft.com/Exchange/mail-flow-best-practices/non-delivery-reports-in-exchange-online/non-delivery-reports-in-exchange-online">Email non-delivery reports in Office 365</A>.

When there's a problem delivering a message, Exchange Server 2013 will send a delivery status notification (DSN) to the message sender. These system-generated messages are also known as bounce messages, and they contain an error code, technical details about the problem, and sometimes troubleshooting steps for the message sender. Non-delivery report (NDR) messages are a common type of status notification. This topic for email administrators describes likely causes and solutions for many NDR status codes. It also tells how to read and interpret NDR messages.

## Common enhanced status codes

The following table contains a list of the enhanced status codes that are returned in NDRs for the most common message delivery failures.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Enhanced status code</th>
<th>Description</th>
<th>Possible cause</th>
<th>Additional information</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>4.3.1</p></td>
<td><p><code>Insufficient system resources</code></p></td>
<td><p>An out-of-memory error occurred. A resource problem, such as a full disk, can cause this problem.</p>
<p>Instead of getting a disk full error, you might be getting an out-of-memory error.</p></td>
<td><p>Ensure that your Exchange server has enough disk storage.</p></td>
</tr>
<tr class="even">
<td><p>4.3.2</p></td>
<td><p><code>System not accepting network messages</code></p></td>
<td><p>This NDR is generated when a queue has been frozen.</p></td>
<td><p>You can resolve this condition by unfreezing the queue.</p></td>
</tr>
<tr class="odd">
<td><p>4.4.1</p></td>
<td><p><code>Connection timed out</code></p></td>
<td><p>The destination server is not responding. Transient network conditions can cause this error. The Exchange server tries automatically to connect to the server again and deliver the mail. If delivery fails after multiple attempts, an NDR with a permanent failure code is generated.</p></td>
<td><p>Monitor the situation. This might be a transient problem that might correct itself.</p></td>
</tr>
<tr class="even">
<td><p>4.4.2</p></td>
<td><p><code>Connection dropped</code></p></td>
<td><p>A connection dropped between the servers. Transient network conditions or a server that is experiencing problems can cause this error. The sending server will retry to deliver the message for a specific time period, and then it will generate further status reports.</p></td>
<td><p>Monitor the situation as the server retries delivery. This might be a transient problem that might correct itself.</p>
<p>This situation can also occur when the message size limit for the connection is reached, or if the message submission rate for the client IP address has exceeded the configured limit.</p></td>
</tr>
<tr class="odd">
<td><p>4.4.7</p></td>
<td><p><code>Message expired</code></p></td>
<td><p>The message in the queue has expired. The sending server tried to relay or deliver the message, but the action was not completed before the message expiration time occurred. This message can also indicate that a message header limit has been reached on a remote server, or some other protocol time-out occurred while communicating with the remote server.</p></td>
<td><p>This message usually indicates an issue on the receiving server. Check the validity of the recipient address, and determine if the receiving server is configured correctly to receive messages.</p>
<p>You might have to reduce the number of recipients in the message header for the host about which you are receiving this error. If you send the message again, it is placed in the queue again. If the receiving server is available, the message is delivered.</p></td>
</tr>
<tr class="even">
<td><p>5.0.0</p></td>
<td><p><code>HELO / EHLO requires domain address</code></p></td>
<td><p>This situation is a permanent failure. Possible causes include:</p>
<ul>
<li><p>There is no route for the given address space; for example, an SMTP connector is configured, but this address does not match.</p></li>
<li><p>DNS returned an authoritative host that was not found for the domain.</p></li>
<li><p>An SMTP error occurred.</p></li>
</ul></td>
<td><p>Some potential resolutions include:</p>
<ul>
<li><p>On one or more SMTP connectors, add an asterisk (<strong>*</strong>) value as the SMTP address space.</p></li>
<li><p>Verify that DNS is working.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>5.1.0</p></td>
<td><p><code>Sender denied</code></p></td>
<td><p>This NDR is caused by a general failure (bad address failure). An email address or another attribute could not be found in Active Directory Domain Services. Contact entries without the <strong>targetAddress</strong> attribute set can cause this problem. Another possible cause could be that the <strong>homeMDB</strong> attribute of a user could not be determined. The <strong>homeMDB</strong> attribute corresponds to the Exchange server on which the user's mailbox resides.</p>
<p>Another common cause of this NDR is when you use Microsoft Outlook to save an email message as a file, and then someone opened the message offline and replied to it. The message property only preserves the <strong>legacyExchangeDN</strong> attribute when Outlook delivers the message, and therefore the lookup could fail.</p></td>
<td><p>Either the recipient address is incorrectly formatted, or the recipient could not be correctly resolved. The first step in resolving this error is to check the recipient address, and send the message again.</p></td>
</tr>
<tr class="even">
<td><p>5.1.1</p></td>
<td><p><code>Bad destination mailbox address</code></p></td>
<td><p>This failure might be caused by the following conditions:</p>
<ul>
<li><p>The recipient's email address was entered incorrectly by the sender.</p></li>
<li><p>No recipient's exists in the destination email system.</p></li>
<li><p>The recipient's mailbox has been moved and the Outlook recipient cache on the sender's computer has not updated.</p></li>
<li><p>An invalid legacy domain name (DN) exists for the recipient's mailbox Active Directory Domain Service.</p></li>
</ul></td>
<td><p>This error typically occurs when the sender of the message incorrectly enters the email address of the recipient. The sender should check the recipient's email address and send again. This error can also occur if the recipient email address was correct in the past but has changed or has been removed from the destination email system.</p>
<p>If the sender of the message is in the same Exchange organization as the recipient, and the recipient's mailbox still exists, determine whether the recipient's mailbox has been relocated to a new email server. If this is the case, Outlook might not have updated the recipient cache correctly. Instruct the sender to remove the recipient's address from sender's Outlook recipient cache and then create a new message. Resending the original message will result in the same failure.</p>
<p>Other issues might cause this error, such as an invalid legacy distinguished name (DN) in Active Directory Domain Services. Examine and correct the former DN of the recipient's mailbox. Then instruct the sender to remove the recipient's address from sender's Outlook recipient cache and then create a new message. Resending the original message will result in the same failure.</p></td>
</tr>
<tr class="odd">
<td><p>5.1.2</p></td>
<td><p><code>Invalid X.400 address</code></p></td>
<td><p>The recipient has a non-SMTP address that can't be matched to a destination. The address does not appear to be local, and there are no connectors configured with address spaces that contain the recipient's address.</p></td>
<td><p>Verify that the recipient's address was entered correctly. If the recipient's address is in a non-SMTP email system that you specifically want to provide mail delivery to, you need to add the appropriate type of connector to your topology and configure it to provide service to the recipient's email system.</p></td>
</tr>
<tr class="even">
<td><p>5.1.3</p></td>
<td><p><code>Invalid recipient address</code></p></td>
<td><p>This message indicates that the recipient's address appears incorrectly on the message.</p></td>
<td><p>Either the recipient's address is formatted incorrectly, or the recipient's address could not be correctly resolved. The first step in resolving this error is to check the recipient's address and send the message again.</p>
<p>Also, examine the SMTP recipient policy, and ensure that each mail domain for which you want to accept mail appears correctly.</p></td>
</tr>
<tr class="odd">
<td><p>5.1.4</p></td>
<td><p><code>Destination mailbox address ambiguous</code></p></td>
<td><p>Two or more recipients in the Exchange organization have the same address.</p></td>
<td><p>This error typically occurs because of a misconfiguration in Active Directory Domain Services. Possibly because of replication problems, two recipient objects in Active Directory Domain Services have the same SMTP address or Exchange Server (EX) address.</p></td>
</tr>
<tr class="even">
<td><p>5.1.7</p></td>
<td><p><code>Invalid address</code></p></td>
<td><p>The sender has a malformed or missing SMTP address, the <strong>mail</strong> attribute in the directory service. The mail item cannot be delivered without a valid <strong>mail</strong> attribute.</p></td>
<td><p>Check the sender directory structure, and determine if the <strong>mail</strong> attribute exists.</p></td>
</tr>
<tr class="odd">
<td><p>5.2.1</p></td>
<td><p><code>Mailbox cannot be accessed</code></p></td>
<td><p>The mailbox cannot be accessed. The mailbox may be offline, disabled, or the message has been quarantined by a rule.</p></td>
<td><p>Check to see if the recipient database is online, the recipient mailbox is disabled, or the message has been quarantined.</p></td>
</tr>
<tr class="even">
<td><p>5.2.2</p></td>
<td><p><code>Mailbox full</code></p></td>
<td><p>The recipient's mailbox has exceeded its storage quota and is no longer able to accept new messages.</p></td>
<td><p>This error occurs when the recipient's mailbox has exceeded its storage quota. The recipient must reduce the size of the mailbox or the administrator must increase the storage quota before delivery can be successful.</p></td>
</tr>
<tr class="odd">
<td><p>5.2.3</p></td>
<td><p><code>Message too large</code></p></td>
<td><p>The message is too large, and the local quota is exceeded. For example, a remote Exchange user might have a restriction on the maximum size of an incoming message.</p></td>
<td><p>Send the message again without attachments, or set the server or the client-side limit to allow a larger message size limit.</p></td>
</tr>
<tr class="even">
<td><p>5.2.4</p></td>
<td><p><code>Mailing list expansion problem</code></p></td>
<td><p>The recipient is a misconfigured dynamic distribution list. Either the filter string or the base DN of the dynamic distribution list is invalid.</p></td>
<td><p>Set the categorizer event logging level to at least the minimum level, and send another message to the dynamic distribution list. Check the application event log for a 6025 event or a 6026 event detailing which attribute is misconfigured on the dynamic distribution list object.</p></td>
</tr>
<tr class="odd">
<td><p>5.3.3</p></td>
<td><p><code>Unrecognized command</code></p></td>
<td><p>When the Exchange remote server reaches capacity of its disk storage to hold mail, it could respond with this NDR. This error usually occurs when the sending server is sending mail with an ESMTP BDAT command. This error also indicates a possible SMTP protocol error.</p></td>
<td><p>Ensure that the remote server has enough storage capacity to hold mail. Check the SMTP log.</p></td>
</tr>
<tr class="even">
<td><p>5.3.4</p></td>
<td><p><code>Message too big for system</code></p></td>
<td><p>The message exceeds a size limit configured on a transport or mailbox database and can't be accepted. This failure can be generated by either the sending email system or the recipient email system.</p></td>
<td><p>This error occurs when the size of the message that was sent by the sender exceeds the maximum allowed message size when passing through a transport component or mailbox database. The sender must reduce the size of the message for the message to be successfully delivered. For more information about how to configure message size limits, see <a href="message-size-limits-exchange-2013-help.md">Message size limits</a>.</p></td>
</tr>
<tr class="odd">
<td><p>5.3.5</p></td>
<td><p><code>System incorrectly configured</code></p></td>
<td><p>A mail-looping situation was detected, which means that the server is configured to loop mail back to itself.</p></td>
<td><p>Check the configuration of the server's connectors for loops, and ensure that each connector is defined by a unique incoming port. If there are multiple virtual servers, ensure that none are set to &quot;All Unassigned.&quot;</p></td>
</tr>
<tr class="even">
<td><p>5.4.4</p></td>
<td><p><code>Invalid arguments</code></p></td>
<td><p>This NDR occurs if no route exists for message delivery, or if the categorizer could not determine the next-hop destination.</p></td>
<td><p>Check that the domain name specified is valid and that a mail exchanger (MX) record exists.</p></td>
</tr>
<tr class="odd">
<td><p>5.4.6</p></td>
<td><p><code>Routing loop detected</code></p></td>
<td><p>A configuration error has caused an email loop. By default, after 20 iterations of an email loop, Exchange interrupts the loop and generates an NDR to the sender of the message.</p></td>
<td><p>This error occurs when the delivery of a message generates another message in response. That message then generates a third message, and the process is repeated, creating a loop. To help protect against exhausting system resources, Exchange interrupts the mail loop after 20 iterations. Mail loops are typically created because of a configuration error on the sending mail server, the receiving mail server, or both. Check the sender's and the recipient's mailbox rules configuration to determine whether automatic message forwarding is enabled.</p></td>
</tr>
<tr class="even">
<td><p>5.5.2</p></td>
<td><p><code>Send hello first</code></p></td>
<td><p>A generic SMTP error occurs when SMTP commands are sent out of sequence. For example, a server attempts to send an AUTH (authorization) command before identifying itself with an EHLO command.</p>
<p>It is possible that this error can also occur when the system disk is full.</p></td>
<td><p>View the SMTP Log or a Netmon trace, and ensure that there is adequate disk storage and virtual memory available.</p></td>
</tr>
<tr class="odd">
<td><p>5.5.3</p></td>
<td><p><code>Too many recipients</code></p></td>
<td><p>The combined total of recipients on the To, Cc, and Bcc lines of the message exceeds the total number of recipients allowed in a single message.</p></td>
<td><p>This error occurs when the sender has included too many recipients on the message. The sender must reduce the number of recipient addresses in the message or the maximum number of recipients must be increased to allow the message to be successfully delivered.</p></td>
</tr>
<tr class="even">
<td><p>5.5.4</p></td>
<td><p><code>Invalid domain name</code></p></td>
<td><p>The message contains either an invalid sender or an incorrect recipient address format.</p>
<p>One possible cause is that the recipient address format might contain characters that are not conforming to Internet standards.</p></td>
<td><p>Check the recipient's address for nonstandard characters.</p></td>
</tr>
<tr class="odd">
<td><p>5.5.6</p></td>
<td><p><code>Invalid message content</code></p></td>
<td><p>This message indicates a possible protocol error.</p></td>
<td><p>Check Event Log for possible failures.</p></td>
</tr>
<tr class="even">
<td><p>5.7.1</p></td>
<td><p><code>Delivery not authorized</code></p></td>
<td><p>The sender of the message is not allowed to send messages to the recipient.</p></td>
<td><p>This error occurs when the sender tries to send a message to a recipient but the sender is not authorized to do this. This frequently occurs when a sender tries to send messages to a distribution group that has been configured to accept messages only from members of that distribution group or other authorized senders. The sender must request permission to send messages to the recipient.</p>
<p>This error can also occur if an Exchange transport rule rejects a message because the message matched conditions that are configured on the transport rule.</p></td>
</tr>
<tr class="odd">
<td><p>5.7.1</p></td>
<td><p><code>Unable to relay</code></p></td>
<td><p>The sending email system is not allowed to send a message to an email system where that email system is not the final destination of the message.</p></td>
<td><p>This error occurs when the sending email system tries to send an anonymous message to a receiving email system, and the receiving email system does not accept messages for the domain or domains specified in one or more of the recipients. The following are the most common reasons for this error:</p>
<ul>
<li><p>A third party tries to use a receiving email system to send spam, and the receiving email system rejects the attempt. By the nature of spam, the sender's email address might have been forged, and the resulting NDR could have been sent to the unsuspecting sender's email address. It is difficult to avoid this situation.</p></li>
<li><p>An MX record for a domain points to a receiving email system where that domain is not accepted. The administrator responsible for the specific domain name must correct the MX record or configure the receiving email system to accept messages sent to that domain, or both.</p></li>
<li><p>A sending email system or client that should use the receiving email system to relay messages does not have the correct permissions to do this.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>5.7.1</p></td>
<td><p><code>Client was not authenticated</code></p></td>
<td><p>The sending email system did not authenticate with the receiving email system. The receiving email system requires authentication before message submission.</p></td>
<td><p>This error occurs when the receiving server must be authenticated before message submission, and the sending email system has not authenticated with the receiving email system. The sending email system administrator must configure the sending email system to authenticate with the receiving email system for delivery to be successful. This error can also occur if you try to accept anonymous messages from the Internet on a Mailbox server that has not been configured to do this.</p></td>
</tr>
<tr class="odd">
<td><p>5.7.3</p></td>
<td><p><code>Not Authorized</code></p></td>
<td><p>The sender prohibited reassignment to the alternate recipient.</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## NDR sections

In Exchange 2013, NDRs are designed to be easy to read and understand by both end users and administrators. Information that is displayed in an NDR is separated into the following two areas:

- A user information section

- A administrator information section

The information in each section is targeted to the readers of that section. The user information section appears first and contains feedback to help the user understand in nontechnical terms why the delivery of the message failed. The **Diagnostic information for administrators** section provides deeper technical information, such as the original message headers, which help email administrators troubleshoot a delivery issue. The following figure shows the user information section and **Diagnostic information for administrators** section of an NDR.

**NDR Sections**

![NDR showing User and Administrator Diagnostic Info](images/Bb232118.96245455-5fb9-4669-a931-5563ddd3ab35(EXCHG.150).png "NDR showing User and Administrator Diagnostic Info")

## User information section

The user information section of an NDR generated by Exchange contains information that you want to communicate to an end user who has sent a message that is later returned with an NDR. The text that is displayed in this section is inserted by the Exchange server that generated the NDR.

The text in the user information section is designed to help end users determine why the message was rejected and how to resend the message successfully if the message should be resent. When applicable, the fully qualified domain name (FQDN) of the server that rejected the message is included in the user information section. If delivery fails to more than one recipient, the email address of each recipient is listed and the reason for the failure is included in the space below the recipient's email address.

You can modify the text in the user information section by using the **New-SystemMessage** cmdlet. By creating a custom message, you can provide specific information to end users, such as a telephone number to use to contact the helpdesk department or a hyperlink to use to obtain self-service support.

## Diagnostic information for administrators

The **Diagnostic information for administrators** section contains more detailed information about the specific error that occurred during delivery of the message, the server that generated the NDR, and the server that rejected the message. The following fields are present in most NDRs and are visible in the "NDR sections" figure earlier in this topic:

- **Generating server**: The generating server is the SMTP server that created the NDR. The generating server takes the enhanced status code that is explained later in this topic. This code creates an easy-to-read NDR. If no remote server is listed below the email address of the sender in the **Diagnostic information for administrators** section, the generating server is also the server that rejected the original email message. If message delivery fails when the message is sent to another recipient in the Exchange organization, the same server typically rejects the original message and generates the NDR.

- **Rejected recipient**: The rejected recipient is the email address of the recipient to which delivery of the original message failed. If delivery to more than one recipient has failed, the email address for each recipient is listed. The rejected recipient field also contains the following subfields for each email address listed:

  - **Remote server**: The remote server field contains the FQDN of the server that rejects delivery of the message during the SMTP conversation. The remote server field is only populated when delivery has been attempted to a remote server, and that delivery attempt has been rejected before the receiving server successfully acknowledges the message after the message body is sent. If the original message is successfully acknowledged by the receiving server and is then rejected because of content restrictions, for example, the remote server field is not populated.

  - **Enhanced status code**: The enhanced status code is the code returned by the server that rejected the original message. The enhanced status code indicates why the original message was rejected. The enhanced status code is not rewritten by Exchange but is used to determine what text to display in the user information section. The enhanced status codes you're most likely to encounter are listed in "Common Enhanced Status Codes" later in this topic. For a detailed list of enhanced status codes, see RFC 3463.

  - **SMTP response**: The SMTP response is the machine readable text returned by the server that rejected the original message. The SMTP response typically contains a short string that provides an explanation of the enhanced status code that is also returned. The SMTP response is not rewritten by Exchange. Additionally, this response is always presented in US-ASCII format.

- **Original message headers**: The original message headers section contains the message headers of the rejected message. These headers can provide useful diagnostic information, such as information that can help you determine the path that the message took before it was rejected or whether the **To** field matches the email address that is specified in the rejected recipient field.

## Examples of NDR messages

The following sections provide examples of two ways that NDR messages can be generated:

- By the same server

- By different servers

## NDR generated and original message rejected by the same server

The following example shows what happens when a remote email organization accepts delivery of an email message through an Edge Transport server, and then rejects that message because of a policy restriction on the recipient's mailbox. In this case, the sender is not allowed to send messages to the recipient. Edge Transport servers do not perform message size validation so the Edge Transport server in this example accepts the message because it has a valid recipient address and the message does not violate another content restrictions. Because the remote email organization accepts the whole message, including the message contents, the remote email organization is responsible for rejecting the message and for generating the NDR message to be sent to the sender.

**NDR generated and message rejected by the same server**

![NDR showing same generating and rejecting server](images/Bb232118.c9a7cd2d-f35f-4d77-8225-c29585fa3ccd(EXCHG.150).gif "NDR showing same generating and rejecting server")

Also, messages that are rejected when they are sent to recipients that are part of the same Exchange organization are typically rejected by the same email server that generates the NDR message. Messages sent to local recipients can be rejected for various reasons, such as mailboxes that have exceeded their quota, lack of authorization to send messages to the recipient address, or hardware failures that result in an extended loss of connectivity to other servers in the organization.

In both situations, no remote server is included under the email address of the recipients listed in the NDR message.

## NDR generated and original message rejected by different servers

The following example shows what happens when a remote email organization rejects delivery of an email message before it ever accepts the message. In this example, the remote server rejects the message and returns an enhanced status code to the local sending server because the specified recipient does not exist. The rejection happens before the receiving server ever acknowledges the message. Because the receiving server doesn't successfully acknowledge the message, the receiving server is not responsible for the message. Therefore, the local sending server generates the NDR message and sends it to the sender of the original message.

**NDR generated and message rejected by different servers**

![NDR showing different generating/sending servers](images/Bb232118.adfb8d5a-9c1d-4cd9-8a71-ce14224434f8(EXCHG.150).gif "NDR showing different generating/sending servers")

## See Also

[What are Exchange NDRs in Exchange Online and Office 365](https://docs.microsoft.com/Exchange/mail-flow-best-practices/non-delivery-reports-in-exchange-online/non-delivery-reports-in-exchange-online)
