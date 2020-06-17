---
title: 'Information Rights Management: Exchange 2013 Help'
TOCTitle: Information Rights Management
ms:assetid: 6ea3a695-3ddd-4d53-b3c6-90041f44ef64
ms:mtpsurl: https://technet.microsoft.com/library/Dd638140(v=EXCHG.150)
ms:contentKeyID: 49319921
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Information Rights Management

_**Applies to:** Exchange Server 2013_

Every day, information workers use e-mail to exchange sensitive information such as financial reports and data, legal contracts, confidential product information, sales reports and projections, competitive analysis, research and patent information, and customer and employee information. Because people can now access their e-mail from just about anywhere, mailboxes have transformed into repositories containing large amounts of potentially sensitive information. As a result, information leakage can be a serious threat to organizations. To help prevent information leakage, Microsoft Exchange Server 2013 includes Information Rights Management (IRM) features, which provide persistent online and offline protection of e-mail messages and attachments.

## What is information leakage?

Leakage of potentially sensitive information can be costly for an organization and have wide-ranging impact on the organization and its business, employees, customers, and partners. Local and industry regulations increasingly govern how certain types of information are stored, transmitted, and secured. To avoid violating applicable regulations, organizations must protect themselves against intentional, inadvertent, or accidental information leakage.

The following are some consequences resulting from information leakage:

- **Financial damage**: Depending on the size, industry, and local regulations, information leakage may result in financial impact due to loss of business or due to fines and punitive damages imposed by courts or regulatory authorities. Public companies may also risk losing market capitalization due to adverse media coverage.

- **Damage to image and credibility**: Information leakage can damage an organization's image and credibility with customers. Moreover, depending on the nature of communication, leaked e-mail messages can potentially be a source of embarrassment for the sender and the organization.

- **Loss of competitive advantage**: One of the most serious threats from information leakage is the loss of competitive advantage in business. Disclosure of strategic plans or disclosure of merger and acquisition information can potentially lead to loss of revenue or market capitalization. Other threats include loss of research information, analytical data, and other intellectual property.

## Traditional solutions to information leakage

Although traditional solutions to information leakage may protect initial access to data, they often don't provide constant protection. The following table lists some traditional solutions and their limitations.

### Traditional solutions

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Solution</th>
<th>Description</th>
<th>Limitations</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Transport Layer Security (TLS)</p></td>
<td><p>TLS is an Internet standard protocol used to secure communications over a network by means of encryption. In a messaging environment, TLS is used to secure server/server and client/server communications.</p>
<p>By default, Exchange 2010 uses TLS for all internal message transfers. Opportunistic TLS is also enabled by default for sessions with external hosts. Exchange first attempts to use TLS encryption for the session, but if a TLS connection can't be established with the destination server, Exchange uses SMTP. You can also configure domain security to enforce mutual TLS with external organizations.</p></td>
<td><p>TLS only protects the SMTP session between two SMTP hosts. In other words, TLS protects information in motion, and it doesn't provide protection at the message-level or for information at rest. Unless the messages are encrypted using another method, messages in the sender's and recipients' mailboxes remain unprotected. For e-mail sent outside the organization, you can require TLS only for the first hop. After a remote SMTP host outside your organization receives the message, it can relay it to another SMTP host over an unencrypted session. Because TLS is a transport layer technology, it can't provide control over what the recipient does with the message.</p></td>
</tr>
<tr class="even">
<td><p>E-mail encryption</p></td>
<td><p>Users can use technologies such as S/MIME to encrypt messages.</p></td>
<td><p>Users decide whether a message gets encrypted. There are additional costs of a public key infrastructure (PKI) deployment, with the accompanying overhead of certificate management for users and protection of private keys. After a message is decrypted, there's no control over what the recipient can do with the information. Decrypted information can be copied, printed, or forwarded. By default, saved attachments aren't protected.</p>
<p>Messages encrypted using technologies such as S/MIME can't be accessed by your organization. The organization can't inspect message content, and therefore can't enforce messaging policies, scan messages for viruses or malicious content, or take any other action that requires accessing the content.</p></td>
</tr>
</tbody>
</table>

Finally, traditional solutions often lack enforcement tools that apply uniform messaging policies to prevent information leakage. For example, a user sends a message containing sensitive information and marks it as **Company Confidential** and **Do Not Forward**. After the message is delivered to the recipient, the sender or the organization no longer has control over the information. The recipient can willfully or inadvertently forward the message (using features such as automatic forwarding rules) to external e-mail accounts, subjecting your organization to substantial information leakage risks.

## IRM in Exchange 2013

In Exchange 2013, you can use IRM features to apply persistent protection to messages and attachments. IRM uses Active Directory Rights Management Services (AD RMS), an information protection technology in Windows Server 2008 and later. With the IRM features in Exchange 2013, your organization and your users can control the rights recipients have for e-mail. IRM also helps allow or restrict recipient actions such as forwarding a message to other recipients, printing a message or attachment, or extracting message or attachment content by copying and pasting. IRM protection can be applied by users in Microsoft Outlook or Microsoft Office Outlook Web App, or it can be based on your organization's messaging policies and applied using transport protection rules or Outlook protection rules. Unlike other e-mail encryption solutions, IRM also allows your organization to decrypt protected content to enforce policy compliance.

AD RMS uses extensible rights markup language (XrML)-based certificates and licenses to certify computers and users, and to protect content. When content such as a document or a message is protected using AD RMS, an XrML license containing the rights that authorized users have to the content is attached. To access IRM-protected content, AD RMS-enabled applications must procure a use license for the authorized user from the AD RMS cluster.

> [!NOTE]
> In Exchange 2013, the Prelicensing agent attaches a use license to messages protected using the AD&nbsp;RMS cluster in your organization. For more details, see Prelicensing later in this topic.

Applications used to create content must be RMS-enabled to apply persistent protection to content using AD RMS. Microsoft Office applications, such as Word, Excel, PowerPoint and Outlook are RMS-enabled and can be used to create and consume protected content.

IRM helps you do the following:

- Prevent an authorized recipient of IRM-protected content from forwarding, modifying, printing, faxing, saving, or cutting and pasting the content.

- Protect supported attachment file formats with the same level of protection as the message.

- Support expiration of IRM-protected messages and attachments so they can no longer be viewed after the specified period.

- Prevent IRM-protected content from being copied using the Snipping Tool in Microsoft Windows.

However, IRM can't prevent information from being copied using the following methods:

- Third-party screen capture programs

- Use of imaging devices such as cameras to photograph IRM-protected content displayed on the screen

- Users remembering or manually transcribing the information

To learn more about AD RMS, see [Active Directory Rights Management Services](https://go.microsoft.com/fwlink/p/?linkid=129823).

## AD RMS rights policy templates

AD RMS uses XrML-based rights policy templates to allow compatible IRM-enabled applications to apply consistent protection policies. In Windows Server 2008 and later, the AD RMS server exposes a Web service that can be used to enumerate and acquire templates. Exchange 2013 ships with the Do Not Forward template. When the Do Not Forward template is applied to a message, only the recipients addressed in the message can decrypt the message. The recipients can't forward the message, copy content from the message, or print the message. You can create additional RMS templates on the AD RMS server in your organization to meet your IRM protection requirements.

IRM protection is applied by applying an AD RMS rights policy template. Using policy templates, you can control permissions that recipients have on a message. Actions such as replying, replying to all, forwarding, extracting information from a message, saving a message, or printing a message can be controlled by applying the appropriate rights policy template to the message.

For more information about rights policy templates, see [AD RMS Policy Template Considerations](https://go.microsoft.com/fwlink/p/?linkid=179455).

For more information about creating AD RMS rights policy templates, see [AD RMS Rights Policy Templates Deployment Step-by-Step Guide](https://go.microsoft.com/fwlink/p/?linkid=136593).

## Applying IRM protection to messages

In Exchange 2010, IRM protection can be applied to messages using the following methods:

- **Manually by Outlook users**: Your Outlook users can IRM-protect messages with the AD RMS rights policy templates available to them. This process uses the IRM functionality in Outlook, and not Exchange. However, you can use Exchange to access messages, and you can take actions (such as applying transport rules) to enforce your organization's messaging policy. For more information about using IRM in Outlook, see [Introduction to IRM for email messages](https://go.microsoft.com/fwlink/p/?linkid=166897).

- **Manually by Outlook Web App users**: When you enable IRM in Outlook Web App, users can IRM-protect messages they send, and view IRM-protected messages they receive. In Exchange 2013 Cumulative Update 1 (CU1), Outlook Web App users can also view IRM-protected attachments using Web-Ready Document Viewing. For more information about IRM in Outlook Web App, see [Information Rights Management in Outlook Web App](information-rights-management-in-outlook-web-app-exchange-2013-help.md).

- **Manually by Windows Mobile and Exchange ActiveSync device users**: In the release to manufacturing (RTM) version of Exchange 2010, users of Windows Mobile devices can view and create IRM-protected messages. This requires users to connect their supported Windows Mobile devices to a computer and activate them for IRM. In Exchange 2010 SP1, you can enable IRM in Microsoft Exchange ActiveSync to allow users of Exchange ActiveSync devices (including Windows Mobile devices) to view, reply to, forward, and create IRM-protected messages. For more information about IRM in Exchange ActiveSync, see [Information Rights Management in Exchange ActiveSync](information-rights-management-in-exchange-activesync-exchange-2013-help.md).

- **Automatically in Outlook 2010 and later**: You can create Outlook protection rules to automatically IRM-protect messages in Outlook 2010 and later. Outlook protection rules are deployed automatically to Outlook 2010 clients, and IRM-protection is applied by Outlook 2010 when the user is composing a message. For more information about Outlook protection rules, see [Outlook protection rules](outlook-protection-rules-exchange-2013-help.md).

- **Automatically on Mailbox servers**: You can create transport protection rules to automatically IRM-protect messages on Exchange 2013 Mailbox servers. For more information about transport protection rules, see [Transport protection rules](transport-protection-rules-exchange-2013-help.md).

    > [!NOTE]
    > IRM protection isn't applied again to messages that are already IRM-protected. For example, if a user IRM-protects a message in Outlook or Outlook Web App, IRM protection isn't applied to the message using a transport protection rule.

## Scenarios for IRM protection

Scenarios for IRM protection are described in the following table.

### Scenarios for IRM protection

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Sending IRM-protected messages</th>
<th>Supported</th>
<th>Requirements</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Within the same on-premises Exchange 2013 deployment</p></td>
<td><p>Yes</p></td>
<td><p>For requirements, see IRM Requirements later in this topic.</p></td>
</tr>
<tr class="even">
<td><p>Between different forests in an on-premises deployment</p></td>
<td><p>Yes</p></td>
<td><p>For requirements, see <a href="https://go.microsoft.com/fwlink/p/?linkid=199009">Configuring AD RMS to Integrate with Exchange Server 2010 Across Multiple Forests</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Between an on-premises Exchange 2013 deployment and a cloud-based Exchange organization</p></td>
<td><p>Yes</p></td>
<td><ul>
<li><p>Use an on-premises AD RMS server.</p></li>
<li><p>Export the trusted publishing domain from your on-premises AD RMS server.</p></li>
<li><p>Import the trusted publishing domain in your cloud-based organization.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>To external recipients</p></td>
<td><p>No</p></td>
<td><p>Exchange 2010 doesn't include a solution for sending IRM-protected messages to external recipients in a non-federated organization. AD RMS offers solutions using trust policies. You can configure a trust policy between your AD RMS cluster and Microsoft account (formerly known as Windows Live ID). For messages sent between two organizations, you can create a federated trust between the two Active Directory forests using Active Directory Federation Services (AD FS). To learn more, see <a href="https://go.microsoft.com/fwlink/p/?linkid=182909">Understanding AD RMS Trust Policies</a>.</p></td>
</tr>
</tbody>
</table>

## Decrypting IRM-protected messages to enforce messaging policies

To enforce messaging policies and for regulatory compliance, you must be able to access encrypted message content. To meet eDiscovery requirements due to litigation, regulatory audits, or internal investigations, you must also be able to search encrypted messages. To help with these tasks, Exchange 2013 includes the following IRM features:

- **Transport decryption**: To apply messaging policies, transport agents such as the Transport Rules agent should have access to message content. Transport decryption allows transport agents installed on Exchange 2013 servers to access message content. For more information, see [Transport decryption](transport-decryption-exchange-2013-help.md).

- **Journal report decryption**: To meet compliance or business requirements, organizations can use journaling to preserve messaging content. The Journaling agent creates a journal report for messages subject to journaling and includes metadata about the message in the report. The original message is attached to the journal report. If the message in a journal report is IRM-protected, journal report decryption attaches a cleartext copy of the message to the journal report. For more information, see [Journal report decryption](journal-report-decryption-exchange-2013-help.md).

- **IRM decryption for Exchange Search**: With IRM decryption for Exchange Search, Exchange Search can index content in IRM-protected messages. When a discovery manager performs an In-Place eDiscovery search, IRM-protected messages that have been indexed are returned in search results. For more information, see [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery).

  > [!NOTE]
  > In Exchange 2010 SP1 and later, members of the Discovery Management role group can access IRM-protected messages returned by a discovery search and residing in a discovery mailbox. To enable this functionality, use the <EM>EDiscoverySuperUserEnabled</EM> parameter with <A href="https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration">Set-IRMConfiguration</A> cmdlet. For more information, see <A href="configure-irm-for-exchange-search-and-https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery">Configure IRM for Exchange Search and In-Place eDiscovery</A>.

To enable these decryption features, Exchange servers must have access to the message. This is accomplished by adding the Federation mailbox, a system mailbox created by Exchange Setup, to the super users group on the AD RMS server. For details, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

## Prelicensing

To view IRM-protected messages and attachments, Exchange 2013 automatically attaches a prelicense to protected messages. This prevents the client from having to make repeated trips to the AD RMS server to retrieve a use license, and enables offline viewing of IRM-protected messages and attachments. Prelicensing also allows IRM-protected messages to be viewed in Outlook Web App. When you enable IRM features, prelicensing is enabled by default.

## IRM agents

In Exchange 2013, IRM functionality is enabled using transport agents in the Transport service on Mailbox servers. IRM agents are installed by Exchange Setup on a Mailbox server. You can't control IRM agents using the management tasks for transport agents.

> [!NOTE]
> In Exchange 2013, IRM agents are built-in agents. Built-in agents aren't included in the list of agents returned by the <STRONG>Get-TransportAgent</STRONG> cmdlet. For more information, see <A href="transport-agents-exchange-2013-help.md">Transport agents</A>.

The following table lists the IRM agents implemented in the Transport service on Mailbox servers.

### IRM agents in the Transport service on Mailbox servers

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Agent</th>
<th>Event</th>
<th>Function</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>RMS Decryption agent</p></td>
<td><p>OnEndOfData (SMTP) and OnSubmittedMessage</p></td>
<td><p>Decrypts messages to allow access to transport agents.</p></td>
</tr>
<tr class="even">
<td><p>Transport Rules agent</p></td>
<td><p>OnRoutedMessage</p></td>
<td><p>Flags messages that match rule conditions in a transport protection rule to be IRM-protected by the RMS Encryption agent.</p></td>
</tr>
<tr class="odd">
<td><p>RMS Encryption agent</p></td>
<td><p>OnRoutedMessage</p></td>
<td><p>Applies IRM protection to messages flagged by the Transport Rules agent and re-encrypts transport decrypted messages.</p></td>
</tr>
<tr class="even">
<td><p>Prelicensing agent</p></td>
<td><p>OnRoutedMessage</p></td>
<td><p>Attaches a prelicense to IRM-protected messages.</p></td>
</tr>
<tr class="odd">
<td><p>Journal Report Decryption agent</p></td>
<td><p>OnCategorizedMessage</p></td>
<td><p>Decrypts IRM-protected messages attached to journal reports and embeds cleartext versions along with the original encrypted messages.</p></td>
</tr>
</tbody>
</table>

For more information about transport agents, see [Transport agents](transport-agents-exchange-2013-help.md).

## IRM requirements

To implement IRM in your Exchange 2013 organization, your deployment must meet the requirements described in the following table.

### IRM requirements

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Server</th>
<th>Requirements</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>AD RMS cluster</p></td>
<td><ul>
<li><p><strong>Operating system</strong>   Windows Server 2012, Windows Server 2008 R2 or Windows Server 2008 SP2 with the hotfix <a href="https://support.microsoft.com/help/973247">Active Directory Rights Management Services role in Windows Server 2008</a> is required.</p></li>
<li><p><strong>Service connection point</strong>   Exchange 2010 and AD RMS-aware applications use the service connection point registered in Active Directory to discover an AD RMS cluster and URLs. AD RMS allows you to register the service connection point from within AD RMS Setup. If the account used to set up AD RMS isn't a member of the Enterprise Admins security group, service connection point registration can be performed after setup is complete. There is only one service connection point for AD RMS in an Active Directory forest.</p></li>
<li><p><strong>Permissions</strong>   Read and Execute permissions to the AD RMS server certification pipeline (<code>ServerCertification.asmx</code> file on AD RMS servers) must be assigned to the following:</p>
<ul>
<li><p>Exchange Servers group or individual Exchange servers</p></li>
<li><p>AD RMS Service group on AD RMS servers</p></li>
</ul>
<p>By default, the ServerCertification.asmx file is located in the <code>\inetpub\wwwroot\_wmcs\certification\</code> folder on AD RMS servers. For details, see <a href="https://go.microsoft.com/fwlink/p/?linkid=186951">Set Permissions on the AD RMS Server Certification Pipeline</a>.</p></li>
<li><p><strong>AD RMS super users</strong>   To enable transport decryption, journal report decryption, IRM in Outlook Web App, and IRM for Exchange Search, you must add the Federation mailbox, a system mailbox created by Exchange 2013 Setup, to the super users group on the AD RMS cluster. For details, see <a href="add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md">Add the Federation Mailbox to the AD RMS Super Users Group</a>.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Exchange</p></td>
<td><ul>
<li><p>Exchange 2010 or later is required.</p></li>
<li><p>The hotfix <a href="https://support.microsoft.com/help/973136">FIX: ArgumentNullException exception error message when a .NET Framework 2.0 SP2-based application tries to process a response with zero-length content to an asynchronous ASP.NET Web service request: &quot;Value cannot be null&quot;</a> is recommended for Microsoft .NET Framework 2.0 SP2.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Outlook</p></td>
<td><ul>
<li><p>Users can IRM-protect messages in Outlook. Beginning with Outlook 2003, AD RMS templates for IRM-protecting messages is supported.</p></li>
<li><p>Outlook protection rules are an Exchange 2010 and Outlook 2010 feature. Previous versions of Outlook don't support this feature.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Exchange ActiveSync</p></td>
<td><ul>
<li><p>Devices supporting Exchange ActiveSync protocol version 14.1, including Windows Mobile devices, can support IRM in Exchange ActiveSync. The mobile e-mail application on a device must support the <strong>RightsManagementInformation</strong> tag defined in Exchange ActiveSync protocol version 14.1. In Exchange 2013, IRM in Exchange ActiveSync allows users with supported devices to view, reply to, forward, and create IRM-protected messages without requiring the user to connect the device to a computer and activate it for IRM. For details, see <a href="information-rights-management-in-exchange-activesync-exchange-2013-help.md">Information Rights Management in Exchange ActiveSync</a>.</p></li>
</ul></td>
</tr>
</tbody>
</table>

> [!NOTE]
> <EM>AD&nbsp;RMS</EM> <EM>cluster</EM> is the term used for an AD&nbsp;RMS deployment in an organization, including a single server deployment. AD&nbsp;RMS is a Web service. It doesn't require that you set up a Windows Server failover cluster. For high availability and load-balancing, you can deploy multiple AD&nbsp;RMS servers in the cluster and use Network Load Balancing.

> [!IMPORTANT]
> In a production environment, installing AD&nbsp;RMS and Exchange on the same server isn't supported.

Exchange 2013 IRM features support Microsoft Office file formats. You can extend IRM protection to other file formats by deploying custom protectors. For more information about custom protectors, see Information Protection and Control Partners in the [Microsoft Partner Center](https://www.microsoft.com/solution-providers/).

## Configuring and testing IRM

You must use the Exchange Management Shell to configure IRM features in Exchange 2013. To configure individual IRM features, use the [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration) cmdlet. You can enable or disable IRM for internal messages, transport decryption, journal report decryption, Exchange Search, and Outlook Web App. For more information about configuring IRM features, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

After you set up an Exchange 2013 server, you can use the [Test-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Test-IRMConfiguration) cmdlet to perform end-to-end tests of your IRM deployment. These tests are useful to verify IRM functionality immediately after initial IRM configuration and on an ongoing basis. The cmdlet performs the following tests:

- Inspects IRM configuration for your Exchange 2013 organization.

- Checks the AD RMS server for version and hotfix information.

- Verifies whether an Exchange server can be activated for RMS by retrieving a Rights Account Certificate (RAC) and client licensor certificate.

- Acquires AD RMS rights policy templates from the AD RMS server.

- Verifies that the specified sender can send IRM-protected messages.

- Retrieves a super user use license for the specified recipient.

- Acquires a prelicense for the specified recipient.

## Extend Rights Management with the Rights Management connector

The Microsoft Rights Management connector (RMS connector) is an optional application that enhances data protection for your Exchange 2013 server by employing cloud-based Microsoft Rights Management services. Once you install the RMS connector, it provides continuous data protection during the lifespan of the information and because these services are customizable, you can define the level of protection you need. For example, you can limit email message access to specific users or set view-only rights for certain messages.

To learn more about the RMS connector and how to install it, see [Rights Management connector](https://docs.microsoft.com/azure/information-protection/deploy-rms-connector).
