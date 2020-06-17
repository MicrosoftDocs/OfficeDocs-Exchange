---
title: 'Edge Subscription credentials: Exchange 2013 Help'
TOCTitle: Edge Subscription credentials
ms:assetid: 1d291bc1-9c00-4d1b-8da0-cb81f63d8305
ms:mtpsurl: https://technet.microsoft.com/library/Bb266959(v=EXCHG.150)
ms:contentKeyID: 61200277
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Edge Subscription credentials

_**Applies to:** Exchange Server 2013_

This topic explains how the Edge Subscription process provisions credentials used to help secure the EdgeSync synchronization process and how EdgeSync uses those credentials to establish a secure LDAP connection between an Exchange 2013 Mailbox server and an Edge Transport server. To learn more about the Edge Subscription process, see [Edge Subscriptions](edge-subscriptions-exchange-2013-help.md).

## Edge Subscription process

The Edge Transport server is subscribed to an Active Directory site to establish a synchronization relationship between the Mailbox servers in an Active Directory site and the subscribed Edge Transport server. The credentials provisioned during the Edge Subscription process are used to help secure the LDAP connection between a Mailbox server and an Edge Transport server in the perimeter network.

When you run the **New-EdgeSubscription** cmdlet on an Edge Transport server, EdgeSync bootstrap replication account (ESBRA) credentials are created in the Active Directory Lightweight Directory Services (AD LDS) directory on the local server and then written to the Edge Subscription file. These credentials are used only to establish initial synchronization and will expire 24 hours after the Edge Subscription file is created. If the Edge Subscription process isn't completed within 24 hours, you will need to run the **New-EdgeSubscription** cmdlet again to create a new Edge Subscription file. The Edge Subscription XML file stores configuration data for the Edge Subscription.

The Edge Subscription XML file contains the data shown in the following table.

### Edge Subscription file contents

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Subscription data</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>EdgeServerName</strong></p></td>
<td><p>The NetBIOS name of the Edge Transport server. The Active Directory name of the Edge Subscription will match this name.</p></td>
</tr>
<tr class="even">
<td><p><strong>EdgeServerFQDN</strong></p></td>
<td><p>The fully qualified domain name (FQDN) of the Edge Transport server. Mailbox servers in the subscribed Active Directory site must be able to locate the Edge Transport server by using DNS to resolve the FQDN.</p></td>
</tr>
<tr class="odd">
<td><p><strong>EdgeCertificateBlob</strong></p></td>
<td><p>The public key of the Edge Transport server's self-signed certificate.</p></td>
</tr>
<tr class="even">
<td><p><strong>ESRAUsername</strong></p></td>
<td><p>The name assigned to the ESBRA. The ESBRA account has the following format: ESRA.<em>Edge Transport server name</em>. ESRA means EdgeSync replication account; note the difference between ESBRA (initial bootstrap replication account) and ESRA.</p></td>
</tr>
<tr class="odd">
<td><p><strong>ESRAPassword</strong></p></td>
<td><p>The password assigned to the ESBRA. The password is generated using a random number generator and is stored in the Edge Subscription file in clear text.</p></td>
</tr>
<tr class="even">
<td><p><strong>EffectiveDate</strong></p></td>
<td><p>The creation date of the Edge Subscription file.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Duration</strong></p></td>
<td><p>The length of time these credentials will be valid before they expire. The ESBRA account is valid for only 24 hours.</p></td>
</tr>
<tr class="even">
<td><p><strong>AdamSslPort</strong></p></td>
<td><p>The secure LDAP port EdgeSync binds to when synchronizing data from Active Directory to AD LDS. By default, this is TCP port 50636.</p></td>
</tr>
<tr class="odd">
<td><p><strong>ProductID</strong></p></td>
<td><p>The licensing information for the Edge Transport server. You need to license the Edge Transport server before creating the Edge Subscription.</p></td>
</tr>
<tr class="even">
<td><p><strong>VersionNumber</strong></p></td>
<td><p>The version number of the Edge Subscription file.</p></td>
</tr>
<tr class="odd">
<td><p><strong>SerialNumber</strong></p></td>
<td><p>The Exchange version of the Edge Transport server.</p></td>
</tr>
</tbody>
</table>

> [!IMPORTANT]
> ESBRA credentials are written to the Edge Subscription file in clear text. You need to protect this file throughout the subscription process. After the Edge Subscription file is imported to your Exchange organization, you should immediately delete the Edge Subscription file from the Edge Transport server, from the network share you used to import the file to your Exchange organization, and from any removable media.

## EdgeSync replication accounts

EdgeSync replication accounts (ESRA) are an important part of EdgeSync security. Authentication and authorization of the ESRA is the mechanism used to help secure the connection between an Edge Transport server and a Mailbox server.

The ESBRA contained in the Edge Subscription file is used to establish a secure LDAP connection during initial synchronization. After the Edge Subscription file is imported to a Mailbox server in the Active Directory site where the Edge Transport server is being subscribed, additional ESRA accounts are created in Active Directory for each Edge Transport-Mailbox server pair. During initial synchronization, the newly created ESRA credentials are replicated to AD LDS. These ESRA credentials are used to help secure later synchronization sessions.

Each EdgeSync replication account is assigned the properties shown in the following table.

### Ms-Exch-EdgeSyncCredential properties

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Property name</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>TargetServerFQDN</strong></p></td>
<td><p>String</p></td>
<td><p>The Edge Transport server accepting these credentials.</p></td>
</tr>
<tr class="even">
<td><p><strong>SourceServerFQDN</strong></p></td>
<td><p>String</p></td>
<td><p>The Mailbox server presenting these credentials. This value is empty if the credential is the bootstrap credential.</p></td>
</tr>
<tr class="odd">
<td><p><strong>EffectiveTime</strong></p></td>
<td><p>DateTime (UTC)</p></td>
<td><p>When to start using this credential.</p></td>
</tr>
<tr class="even">
<td><p><strong>ExpirationTime</strong></p></td>
<td><p>DateTime (UTC)</p></td>
<td><p>When to stop using this credential.</p></td>
</tr>
<tr class="odd">
<td><p><strong>UserName</strong></p></td>
<td><p>String</p></td>
<td><p>The username used to authenticate.</p></td>
</tr>
<tr class="even">
<td><p><strong>Password</strong></p></td>
<td><p>Byte</p></td>
<td><p>The password used to authenticate. The password is encrypted using <strong>ms-Exch-EdgeSync-Certificate</strong>.</p></td>
</tr>
</tbody>
</table>

The following sections describe how the ESRA credentials are provisioned and used during the EdgeSync synchronization process.

## Provision the EdgeSync bootstrap replication account

When the **New-EdgeSubscription** cmdlet is run on the Edge Transport server, the ESBRA is provisioned as follows:

- A self-signed certificate (Edge-Cert) is created on the Edge Transport server. The private key is stored in the local computer store and the public key is written to the Edge Subscription file.

- The ESBRA account is created in AD LDS, and its credentials are written to the Edge Subscription file.

- The Edge Subscription file is exported by copying it to removable media (because the Edge Server is not in your Active Directory, you cannot use a shared folder for exporting the file). The file is now ready to import to a Mailbox server.

## Provision EdgeSync replication accounts in Active Directory

When the Edge Subscription file is imported on a Mailbox server, the following steps occur to establish a record of the Edge Subscription in Active Directory and to provision additional ESRA credentials:

1. An Edge Transport server configuration object is created in Active Directory. The Edge-Cert certificate is written to this object as an attribute.

2. Every Mailbox server in the subscribed Active Directory site receives an Active Directory notification that a new Edge Subscription has been registered. As soon as the notification is received, each Mailbox server retrieves the ESRA.edge account and encrypts the account by using the Edge-Cert public key. The encrypted ESRA.edge account is written to the Edge Transport server configuration object.

3. Each Mailbox server creates a self-signed certificate (TransportService-Cert). The private key is stored in the local computer store and the public key is stored in the Mailbox server configuration object in Active Directory.

4. Each Mailbox server encrypts the ESRA.edge account by using the public key of its own TransportService certificate and then stores it in its own configuration object.

5. Each Mailbox server generates an ESRA for each existing Edge Transport server configuration object in Active Directory (ESRA.edge. *Mailboxname.\#*).

   Example: ESRA.edge.Example.0

   The password for ESRA.edge is generated by a random number generator and is encrypted by using the public key of the TransportService-Cert certificate. The generated password has the maximum length allowed for Windows Server.

6. Each ESRA.edge. *Mailboxname.\#* account is encrypted by using the public key of the Edge-Cert certificate and is stored on the Edge Transport server configuration object in Active Directory.

The following sections explain how these accounts are used during EdgeSync synchronization.

## Authenticate initial replication

The initial ESBRA account is used only when establishing initial synchronization. During the first EdgeSync synchronization, the additional ESRA accounts, ESRA.edge.*Mailboxname.\#*, are replicated to AD LDS. These accounts are used to authenticate later EdgeSync synchronization sessions.

The Mailbox server that performs the initial replication is determined randomly. The first Mailbox server in the Active Directory site to perform a topology scan and discover the new Edge Subscription performs the initial replication. Because this discovery is based on the timing of the topology scan, any Mailbox server in the site may perform the initial replication.

EdgeSync initiates a secure LDAP session from the Mailbox server to the Edge Transport server. The Edge Transport server presents its self-signed certificate and the Mailbox server verifies that the certificate matches the certificate stored on the Edge Transport server configuration object in Active Directory. After the Edge Transport server's identity is verified, the Mailbox server provides the credentials of the ESRA.edge.*Mailboxname.\#* account to the Edge Transport server. The Edge Transport server verifies the credentials against the account stored in AD LDS.

The EdgeSync service on the Mailbox server then pushes the topology, configuration, and recipient data from Active Directory to AD LDS. The change to the Edge Transport server configuration object in Active Directory is replicated to AD LDS. AD LDS receives the newly added ESRA.edge.*Mailboxname.\#* entries and the Microsoft Exchange Credential Service creates the corresponding AD LDS account. These accounts are now available to authenticate later scheduled EdgeSync synchronization sessions.

## Microsoft Exchange Credential Service

The Microsoft Exchange Credential Service is part of the Edge Subscription process. The Credential Service runs only on the Edge Transport server. This service creates the reciprocal ESRA accounts in AD LDS so a Mailbox server can authenticate to an Edge Transport server to perform EdgeSync synchronization. EdgeSync doesn't communicate directly with the Microsoft Exchange Credential Service. The Microsoft Exchange Credential Service communicates with AD LDS and installs the ESRA credentials whenever the Mailbox server updates them.

## Authenticate scheduled synchronization sessions

After initial EdgeSync synchronization finishes, the EdgeSync synchronization schedule is established and any Active Directory data that has changed is regularly updated in AD LDS. A Mailbox server initiates a secure LDAP session with the AD LDS instance on the Edge Transport server. AD LDS proves its identity to that Mailbox server by presenting its self-signed certificate. The Mailbox server presents its ESRA.edge credentials to AD LDS. The ESRA.edge password is encrypted using the Mailbox server's self-signed certificate's public key. Only that particular Mailbox server can use those credentials to authenticate to AD LDS.

## Renew EdgeSync replication accounts

The password for the ESRA account must comply with the local server's password policy. To prevent the password renewal process from causing temporary authentication failure, a second ESRA.edge account is created seven days before the first ESRA.edge account expires, with an effective time three days before the first ESRA expiration time. As soon as the second ESRA.edge account becomes effective, EdgeSync stops using the first account and starts to use the second account. When the expiration time for the first account is reached, those ESRA credentials are deleted. This renewal process will continue until the Edge Subscription is removed.
