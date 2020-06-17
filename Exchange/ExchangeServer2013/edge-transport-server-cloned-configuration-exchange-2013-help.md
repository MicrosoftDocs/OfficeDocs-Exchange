---
title: 'Edge Transport server cloned configuration: Exchange 2013 Help'
TOCTitle: Edge Transport server cloned configuration
ms:assetid: 683a6b8a-59bf-43ed-96c8-504945c2f665
ms:mtpsurl: https://technet.microsoft.com/library/Aa998622(v=EXCHG.150)
ms:contentKeyID: 61200288
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Edge Transport server cloned configuration

_**Applies to:** Exchange Server 2013_

Edge Transport servers store their configuration information in Active Directory Lightweight Directory Services (AD LDS). You can install more than one Edge Transport server in the perimeter network and use a DNS round robin to help balance network traffic among the Edge Transport servers. Round robin is a simple mechanism used by DNS servers to share and distribute loads for network resources.

To ensure all Edge Transport servers use the same configuration information, use the provided cloned configuration scripts to duplicate your source server's configuration on a target server.

*Cloned configuration* is used to deploy new Edge Transport servers based on a configured source server. Source server configuration information is duplicated and then exported to an XML file. The XML file is then imported to the target server.

This topic provides an overview of the cloned configuration process. For detailed steps about configuring your Edge Transport servers using cloned configuration, see [Configure Edge Transport server using cloned configuration](configure-edge-transport-server-using-cloned-configuration-exchange-2013-help.md).

## Cloned configuration and EdgeSync

Run the EdgeSync process after you import the cloned configuration. To perform recipient lookup and message security tasks, the Edge Transport server requires data that resides in Active Directory. *EdgeSync* is a collection of processes run on an Exchange 2013 Mailbox server to establish one-way replication of recipient and configuration information from Active Directory to the AD LDS instance on an Edge Transport server. EdgeSync copies only information required for the Edge Transport server to perform anti-spam tasks and information about connector configuration required to enable end-to-end mail flow. EdgeSync performs scheduled updates so the information in AD LDS remains current.

Cloned configuration doesn't duplicate a server's Edge Subscription settings. The certificates used by EdgeSync aren't cloned. You must run the EdgeSync process separately for each Edge Transport server. EdgeSync overwrites any settings included in both the cloned configuration information and in EdgeSync replication information. These settings include Send connectors, Receive connectors, accepted domains, and remote domains.

## Cloned configuration process

The cloned configuration process consists of three steps:

1. Export the configuration from the source server.

    Run the ExportEdgeConfig.ps1 script (located in %ExchangeInstallPath%Scripts) to export the source server's configuration information to an intermediate XML file.

2. Validate the configuration on the target server.

    Run the ImportEdgeConfig.ps1 script (located in %ExchangeInstallPath%Scripts). This script checks the existing information in the intermediate XML file to see whether the exported settings are valid for the target server and then creates an answer file. The answer file specifies the server-specific information used when you import the configuration onto the target server. The answer file contains entries for each source server setting that isn't valid for the target server. You can modify these settings so that they're valid for the target server. If all settings are valid, the answer file contains no entries.

3. Import the configuration on the target server.

    The ImportEdgeConfig.ps1 script uses the intermediate XML file and the answer file to clone an existing configuration or to restore the server to a specific configuration.

## Step 1: Export the configuration from the source server

After you install and configure the Edge Transport server role, run the ExportEdgeConfig.ps1 script (located in %ExchangeInsallPath%Scripts). This script retrieves the source server's configuration information and stores the information in an intermediate XML file.

The following information is exported from the source server and stored in the intermediate XML file:

- Transport service-related information and log file path information:

  - *ReceiveProtocolLogPath*

  - *SendProtocolLogPath*

  - *MessageTrackingLogPath*

  - *PickupDirectoryPath*

  - *RoutingTableLogPath*

- Transport agent-related information, including status and priority settings of each transport agent.

- All Send connector-related information. If any Send connectors are configured to use credentials, the password is written to the intermediate XML file as an encrypted string. You can use the *-key* parameter with the ImportEdgeConfig.ps1 and ExportEdgeConfig.ps1 scripts to specify the 32-byte string to use for password encryption and decryption. If you don't use the *-key* parameter, a default encryption key is used.

- Receive connector-related information. To modify local network binding and port properties, you must modify the configuration information in the answer file that's created in the validate configuration step.

- Accepted domain configuration.

- Remote domain configuration.

- Anti-spam features configuration settings:

  - IP Allow list information. Only IP Allow list entries that were manually configured by the administrator are exported.

  - IP Block list information.

  - Content filter configuration.

  - Recipient filter configuration.

  - Address rewrite entries.

  - Attachment filter entries.

## Step 2: Validate the configuration on the target server

The target server is an Exchange 2013 server that has a clean installation of the Edge Transport server role. First, run the ImportEdgeConfig.ps1 script (located at %ExchangeInsallPath%Scripts) on the target server to validate the existing information in the intermediate XML file and to create the answer file. The answer file specifies the server-specific information used when you import the configuration onto the target server. The answer file contains entries for each source server setting that isn't valid for the target server. You will need to modify these settings so that they're valid for the target server. If all settings are valid, the answer file contains no entries. The intermediate XML file can be used for different target servers. The answer file is specific to a target server.

The ImportEdgeConfig.ps1 script (located at %ExchangeInsallPath%Scripts) performs the following tasks:

- Verifies that the data paths and log paths can be created on the target server. If the paths can't be created, a blank path is inserted into the answer file.

- For each Send connector in the XML file, the script adds a blank entry for the source IP address in the answer file.

- For each Receive connector in the XML file, the script adds a blank entry for the local network bindings in the answer file.

You will need to manually modify the answer file to provide the following information about server-specific settings:

- Fill in the data paths and log paths. If these paths are left blank in the answer file, the paths configured in the intermediate XML file will be used in the next step when you import the configuration onto the target server.

- For each Send connector entry, fill in the source IP address. If this field is left blank, an error will occur in the import configuration step.

- For each Receive connector entry, fill in the local network bindings. If the local network bindings are left blank, an error will occur when you try to import the configuration onto the target server.

## Step 3: Import the configuration onto the target server

You can perform this step on any target server to clone the configuration of an existing Edge Transport server or to restore the server to a specific configuration. Run the ImportEdgeConfig.ps1 script (located at %ExchangeInsallPath%Scripts) to validate and import the new configuration. After you run this script, the target server's configuration will match the settings in the intermediate XML file and the answer file.

> [!IMPORTANT]
> We recommend that you back up the existing server configuration before running the import configuration process, so that if the cloning operation fails, you can restore the server to its previous stable state.

This step uses the server-specific information provided in the answer file. If a setting isn't specified in the answer file, the data in the intermediate XML file will be used. Before the script modifies the configuration, the script validates the data in the intermediate XML file and the answer file.

Import configuration modifies the following target server configuration settings:

- The transport agent configuration is modified.

- The existing connectors on the target server are removed, and the connectors present in the intermediate XML file are added.

- The existing accepted domains are removed, and the accepted domain entries in the intermediate XML file are added.

- The existing remote domains are removed, and the remote domain entries in the intermediate XML file are added.

- The existing IP Allow list entries are removed, and the IP Allow list entries in the intermediate remote domains file are added.

- The existing IP Block list entries are removed, and the IP Block list entries in the intermediate remote domains file are added.

- The following anti-spam configuration is cloned to the target server:

  - Content filter configuration

  - Recipient filter configuration

  - Address rewrite entries

  - Attachment filter entries

## Transport configuration information

The settings of the transport configuration object define server-wide email transport settings for an Edge Transport server. When you import the intermediate XML file to the target server, all transport configuration object settings except the following are imported:

- General names and creation dates from the exported XML file

- Send connector information

- Receive connector information

- Attachment filter entries

- The **MaxDumpsterSizePerStorageGroup** attribute entry

After the import process is complete, you may also configure the settings by using the **Set-TransportConfig** cmdlet. For more information, see [Set-TransportConfig](https://docs.microsoft.com/powershell/module/exchange/Set-TransportConfig).

The attributes in the following table are associated with the transport configuration object and the default values. You can configure this object on both Mailbox servers and Edge Transport servers. However, many attributes apply only to the Transport service on Exchange 2013 Mailbox servers. Configuring these attributes on an Edge Transport server will have no effect.

### Transport configuration attributes and default values

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Description</th>
<th>Default value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>ClearCategories</strong></p></td>
<td><p>This attribute specifies whether to clear Microsoft Outlook categories during content conversion.</p></td>
<td><p>True</p></td>
</tr>
<tr class="even">
<td><p><strong>GenerateCopyOfDSNFor</strong></p></td>
<td><p>This attribute specifies the delivery status notification (DSN) codes that cause the DSN message to be copied to the postmaster email address. DSN codes are entered as <em>x.y.z</em> and are separated by commas.</p></td>
<td><p>5.4.8, 5.4.6, 5.4.4, 5.2.4, 5.2.0, 5.1.4</p></td>
</tr>
<tr class="odd">
<td><p><strong>InternalSMTPServers</strong></p></td>
<td><p>This attribute specifies a list of internal SMTP server IP addresses or IP address ranges that should be ignored by Sender ID and connection filtering.</p></td>
<td><p>Null</p></td>
</tr>
<tr class="even">
<td><p><strong>JournalingReportNdrTo</strong></p></td>
<td><p>This attribute specifies the email address to which journal reports are sent if the journaling mailbox is unavailable. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>Null</p></td>
</tr>
<tr class="odd">
<td><p><strong>MaxDumpsterSizePerStorageGroup</strong></p></td>
<td><p>This attribute specifies the maximum size of the transport dumpster on a Mailbox server. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>18 MB</p></td>
</tr>
<tr class="even">
<td><p><strong>MaxDumpsterTime</strong></p></td>
<td><p>This attribute specifies how long an email message should remain in the transport dumpster on a Mailbox server. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>7.00:00:00</p></td>
</tr>
<tr class="odd">
<td><p><strong>MaxReceiveSize</strong></p></td>
<td><p>This attribute specifies the maximum message size that can be received by recipients in the organization. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>10 MB</p></td>
</tr>
<tr class="even">
<td><p><strong>MaxRecipientEnvelopeLimit</strong></p></td>
<td><p>This attribute specifies the maximum number of recipients that are allowed in a single email message. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>5,000</p></td>
</tr>
<tr class="odd">
<td><p><strong>MaxSendSize</strong></p></td>
<td><p>This attribute specifies the maximum message size that can be sent by senders in the organization. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>10 MB</p></td>
</tr>
<tr class="even">
<td><p><strong>TLSReceiveDomainSecureList</strong></p></td>
<td><p>This attribute specifies the remote domains that will use mutual Transport Layer Security (TLS) authentication through Receive connectors configured to support Domain Security. Multiple domains may be separated by commas. The wildcard character (*) isn't supported in the domains that are listed in this attribute.</p></td>
<td><p>Null</p></td>
</tr>
<tr class="odd">
<td><p><strong>TLSSendDomainSecureList</strong></p></td>
<td><p>This attribute specifies the remote domains that will use mutual TLS authentication when email is sent through a Send connector configured to support Domain Security and the address space of the target domain. Multiple domains may be separated by commas. The wildcard character (*) isn't supported in the domains that are listed in this attribute.</p></td>
<td><p>Null</p></td>
</tr>
<tr class="even">
<td><p><strong>VerifySecureSubmitEnabled</strong></p></td>
<td><p>This attribute verifies that email clients that are submitting messages from mailboxes on Mailbox servers are using encrypted MAPI submission. This attribute doesn't apply to the configuration of an Edge Transport server. The valid values for this attribute are <code>$true</code> or <code>$false</code>.</p></td>
<td><p>False</p></td>
</tr>
<tr class="odd">
<td><p><strong>VoicemailJournalingEnabled</strong></p></td>
<td><p>This attribute specifies whether Unified Messaging voice mail is journaled by the Journaling agent. This attribute doesn't apply to the configuration of an Edge Transport server.</p></td>
<td><p>True</p></td>
</tr>
</tbody>
</table>

> [!NOTE]
> If the Edge Transport server is later subscribed to the Exchange organization, the value of the <STRONG>InternalSMTPServers</STRONG> attribute will be overwritten during the EdgeSync process. For more information, see <A href="edge-subscriptions-exchange-2013-help.md">Edge Subscriptions</A>.
