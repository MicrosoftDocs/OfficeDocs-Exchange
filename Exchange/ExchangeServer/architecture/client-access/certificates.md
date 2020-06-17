---
localization_priority: Normal
description: 'Summary: Learn about SSL, TLS, encryption, and digital certificates in Exchange Server.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a9e2e08c-d46a-4135-a387-eb653212b676
title: Digital certificates and encryption in Exchange Server
ms.collection: exchange-server
ms.reviewer:
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Digital certificates and encryption in Exchange Server

Encryption and digital certificates are important considerations in any organization. By default, Exchange Server is configured to use Transport Layer Security (TLS) to encrypt communication between internal Exchange servers, and between Exchange services on the local server. But, Exchange administrators need to consider their encryption requirements for communication with internal and external clients (computers and mobile devices), and external messaging servers.

> [!NOTE]
> Exchange Server 2019 includes important changes to improve the security of client and server connections. The default configuration for encryption will enable TLS 1.2 only and disable support for older algorithms (namely, DES, 3DES, RC2, RC4 and MD5). It will also configure elliptic curve key exchange algorithms with priority over non-elliptic curve algorithms. In Exchange Server 2016 and later, all cryptography settings are inherited from the configuration specified in the operating system. For additional information, see [Exchange Server TLS Guidance](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Exchange-Server-TLS-guidance-part-1-Getting-Ready-for-TLS-1-2/ba-p/607649).

This topic describes the different types of certificates that are available, the default configuration for certificates in Exchange, and recommendations for additional certificates that you'll need to use with Exchange.

For the procedures that are required for certificates in Exchange Server, see [Certificate procedures in Exchange Server](certificate-procedures.md).

## Digital certificates overview

Digital certificates are electronic files that work like an online password to verify the identity of a user or a computer. They're used to create the encrypted channel that's used for client communications. A certificate is a digital statement that's issued by a certification authority (CA) that vouches for the identity of the certificate holder and enables the parties to communicate in a secure manner by using encryption.

Digital certificates provide the following services:

- **Encryption**: They help protect the data that's exchanged from theft or tampering.

- **Authentication**: They verify that their holders (people, web sites, and even network devices such as routers) are truly who or what they claim to be. Typically, the authentication is one-way, where the source verifies the identity of the target, but mutual TLS authentication is also possible.

Certificates can be issued for several uses. For example: web user authentication, web server authentication, Secure/Multipurpose Internet Mail Extensions (S/MIME), Internet Protocol security (IPsec), and code signing.

A certificate contains a public key and attaches that public key to the identity of a person, computer, or service that holds the corresponding private key. The public and private keys are used by the client and the server to encrypt data before it's transmitted. For Windows users, computers, and services, trust in the CA is established when the root certificate is defined in the trusted root certificate store, and the certificate contains a valid certification path. A certificate is considered valid if it hasn't been revoked (it isn't in the CA's certificate revocation list or CRL), or hasn't expired.

The three primary types of digital certificates are described in the following table.

|**Type**|**Description**|**Advantages**|**Disadvantages**|
|:-----|:-----|:-----|:-----|
|Self-signed certificate|The certificate is signed by the application that created it.|Cost (free).|The certificate isn't automatically trusted by client computers and mobile devices. The certificate needs to be manually added to the trusted root certificate store on all client computers and devices, but not all mobile devices allow changes to the trusted root certificate store. <br/><br/> Not all services work with self-signed certificates. <br/><br/> Difficult to establish an infrastructure for certificate lifecycle management. For example, self-signed certificates can't be revoked.|
|Certificate issued by an internal CA|The certificate is issued by a public key infrastructure (PKI) in your organization. An example is Active Directory Certificate Services (AD CS). For more information, see [Active Directory Certificate Services Overview](https://go.microsoft.com/fwlink/p/?LinkID=392697).|Allows organizations to issue their own certificates. <br/><br/> Less expensive than certificates from a commercial CA.|Increased complexity to deploy and maintain the PKI. <br/><br/> The certificate isn't automatically trusted by client computers and mobile devices. The certificate needs to be manually added to the trusted root certificate store on all client computers and devices, but not all mobile devices allow changes to the trusted root certificate store.|
|Certificate issued by a commercial CA|The certificate is purchased from a trusted commercial CA.|Simplified certificate deployment, because all clients, devices, and servers automatically trust the certificates.|Cost. You need to plan ahead to minimize the number of certificates that are required.|

To prove that a certificate holder is who they claim to be, the certificate must accurately identify the certificate holder to other clients, devices, or servers. The three basic methods to do this are described in the following table.

|**Method**|**Description**|**Advantages**|**Disadvantages**|
|:-----|:-----|:-----|:-----|
|Certificate subject match|The certificate's **Subject** field contains the common name (CN) of the host. For example, the certificate that's issued to www.contoso.com can be used for the web site https://www.contoso.com.|Compatible with all clients, devices, and services. <br/><br/> Compartmentalization. Revoking the certificate for a host doesn't affect other hosts.|Number of certificates required. You can only use the certificate for the specified host. For example, you can't use the www.contoso.com certificate for ftp.contoso.com, even when the services are installed on the same server. <br/><br/> Complexity. On a web server, each certificate requires its own IP address binding.|
|Certificate subject alternative name (SAN) match|In addition to the **Subject** field, the certificate's **Subject Alternative Name** field contains a list of multiple host names. For example: <br/>• www.contoso.com <br/>• ftp.contoso.com <br/>• ftp.eu.fabirkam.net|Convenience. You can use the same certificate for multiple hosts in multiple, separate domains. <br/><br/> Most clients, devices, and services support SAN certificates. <br/><br/> Auditing and security. You know exactly which hosts are capable of using the SAN certificate.|More planning required. You need to provide the list of hosts when you create the certificate. <br/><br/> Lack of compartmentalization. You can't selectively revoke certificates for some of the specified hosts without affecting all of the hosts in the certificate.|
|Wildcard certificate match|The certificate's **Subject** field contains the common name as the wildcard character (\*) plus a single domain or subdomain. For example, \*.contoso.com or \*.eu.contoso.com. The \*.contoso.com wildcard certificate can be used for: <br/>• www.contoso.com <br/>• ftp.contoso.com <br/>• mail.contoso.com|Flexibility. You don't need to provide a list of hosts when you request the certificate, and you can use the certificate on any number of hosts that you may need in the future.|You can't use wildcard certificates with other top-level domains (TLDs). For example, you can't use the \*.contoso.com wildcard certificate for \*.contoso.net hosts. <br/><br/> You can only use wildcard certificates for host names at the level of the wildcard. For example, you can't use the \*.contoso.com certificate for www.eu.contoso.com. Or, you can't use the \*.eu.contoso.com certificate for www.uk.eu.contoso.com. <br/><br/> Older clients, devices, applications, or services might not support wildcard certificates. <br/><br/> Wildcards aren't available with Extended Validation (EV) certificates. <br/><br/> Careful auditing and control is required. If the wildcard certificate is compromised, it affects every host in the specified domain.|

## Certificates in Exchange

When you install Exchange 2016 or Exchange 2019 on a server, two self-signed certificates are created and installed by Exchange. A third self-signed certificate is created and installed by Microsoft Windows for the Web Management service in Internet Information Services (IIS). These three certificates are visible in the Exchange admin center (EAC) and the Exchange Management Shell, and are described in the following table:

|**Name**|**Comments**|
|:-----|:-----|
|Microsoft Exchange|This Exchange self-signed certificate has the following capabilities: <br/>• The certificate is automatically trusted by all other Exchange servers in the organization. This includes any Edge Transport servers subscribed to the Exchange organization. <br/>• The certificate is automatically enabled for all Exchange services except Unified Messaging, and is used to encrypt internal communication between Exchange servers, Exchange services on the same computer, and client connections that are proxied from the Client Access services to the backend services on Mailbox servers. (**Note**: UM is not available on Exchange 2019.) <br/>• The certificate is automatically enabled for inbound connections from external SMTP messaging servers, and outbound connections to external SMTP messaging servers. This default configuration allows Exchange to provide *opportunistic TLS* on all inbound and outbound SMTP connections. Exchange attempts to encrypt the SMTP session with an external messaging server, but if the external server doesn't support TLS encryption, the session is unencrypted. <br/>• The certificate doesn't provide encrypted communication with internal or external clients. Clients and servers don't trust the Exchange self-signed certificate, because the certificate isn't defined in their trusted root certification stores.|
|Microsoft Exchange Server Auth Certificate|This Exchange self-signed certificate is used for server-to-server authentication and integration by using OAuth. For more information, see [Plan Exchange Server integration with SharePoint and Skype for Business](../../plan-and-deploy/integration-with-sharepoint-and-skype/integration-with-sharepoint-and-skype.md).|
|WMSVC|This Windows self-signed certificate is used by the Web Management service in IIS to enable remote management of the web server and its associated web sites and applications. <br/> If you remove this certificate, the Web Management service will fail to start if no valid certificate is selected. Having the service in this state can prevent you from installing Exchange updates, or uninstalling Exchange from the server. For instructions on how to correct this issue, see [Event ID 1007 - IIS Web Management Service Authentication](https://go.microsoft.com/fwlink/p/?LinkId=746383)|

The properties of these self-signed certificates are described in the [Properties of the default self-signed certificates](#properties-of-the-default-self-signed-certificates) section.

These are the key issues that you need to consider when it comes to certificates in Exchange:

- You don't need to replace the Microsoft Exchange self-signed certificate to encrypt network traffic between Exchange servers and services in your organization.

- You need additional certificates to encrypt connections to Exchange servers by internal and external clients.

- You need additional certificates to force the encryption of SMTP connections between Exchange servers and external messaging servers.

The following elements of planning and deployment for Exchange Server are important drivers for your certificate requirements:

- **Load balancing**: Do you plan to terminate the encrypted channel at load balancer or reverse proxy server, use Layer 4 or Layer 7 load balancers, and use session affinity or no session affinity? For more information, see [Load Balancing in Exchange 2016](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Load-Balancing-in-Exchange-2016/ba-p/604048).

- **Namespace planning**: What versions of Exchange are present, are you using the bound or unbound namespace model, and are you using *split-brain DNS* (configuring different IP addresses for the same host based on internal vs. external access)? For more information, see [Namespace Planning in Exchange 2016](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Namespace-Planning-in-Exchange-2016/ba-p/604072).

- **Client connectivity**: What services will your clients use (web-based services, POP, IMAP, etc.) and what versions of Exchange are involved? For more information, see the following topics:

  - [Client Connectivity in an Exchange 2016 Coexistence Environment with Exchange 2013](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Client-Connectivity-in-an-Exchange-2016-Coexistence-Environment/ba-p/603925)

  - [Client Connectivity in an Exchange 2016 Coexistence Environment with Exchange 2010](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Client-Connectivity-in-an-Exchange-2016-Coexistence-Environment/ba-p/603945)

  - [Client Connectivity in an Exchange 2016 Coexistence Environment with Mixed Exchange Versions](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Client-Connectivity-in-an-Exchange-2016-Coexistence-Environment/ba-p/604284)

## Certificate requirements for Exchange services

The Exchange services that certificates can be assigned to are described in the following table.

|**Service**|**Description**|
|:-----|:-----|
|IIS (HTTP)|By default, the following services are offered under the default website in the Client Access (frontend) services on a Mailbox server, and are used by clients to connect to Exchange: <br/>• Autodiscover <br/>• Exchange ActiveSync <br/>• Exchange admin center <br/>• Exchange Web Services <br/>• Offline address book (OAB) distribution <br/>• Outlook Anywhere (RPC over HTTP) <br/>• Outlook MAPI over HTTP <br/>• Outlook on the web <br/>• Remote PowerShell<sup>\*</sup> <br/><br/> Because you can only associate a single certificate with a website, all the DNS names that clients use to connect to these services need to be included in the certificate. You can accomplish this by using a SAN certificate or a wildcard certificate.|
|POP or IMAP|The certificates that are used for POP or IMAP can be different from the certificate that's used for IIS. However, to simplify administration, we recommend that you also include the host names that are used for POP or IMAP in your IIS certificate, and use the same certificate for all of these services.|
|SMTP|SMTP connections from clients or messaging servers are accepted by one or more Receive connectors that are configured in the Front End Transport service on the Exchange server. For more information, see [Receive connectors](../../mail-flow/connectors/receive-connectors.md). <br/><br/> To require TLS encryption for SMTP connections, you can use a separate certificate for each Receive connector. The certificate must include the DNS name that's used by the SMTP clients or servers to connect to the Receive connector. To simplify certificate management, consider including all DNS names for which you have to support TLS traffic in a single certificate. <br/><br/> To require *mutual TLS authentication*, where the SMTP connections between the source and destination servers are both encrypted and authenticated, see [Domain Security](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124392(v=exchg.141)).|
|Unified Messaging (UM)|For more information, see [Deploying Certificates for UM](https://docs.microsoft.com/exchange/deploying-certificates-for-um-exchange-2013-help). <br/> **Note**: UM is not available in Exchange 2019.|
|Hybrid deployment with Microsoft Office 365|For more information, see [Certificate Requirements for Hybrid Deployments](https://docs.microsoft.com/exchange/certificate-requirements).|
|Secure/Multipurpose Internet Mail Extensions (S/MIME)|For more information, see [S/MIME for message signing and encryption](../../policy-and-compliance/smime.md).|

<sup>\*</sup> Kerberos authentication and Kerberos encryption are used for remote PowerShell access, from both the Exchange admin center and the Exchange Management Shell. Therefore, you don't need to configure your certificates for use with remote PowerShell, as long as you connect directly to an Exchange server (not to a load balanced namespace). To use remote PowerShell to connect to an Exchange server from a computer that isn't a member of the domain, or to connect from the Internet, you need to configure your certificates for use with remote PowerShell.

## Best practices for Exchange certificates

Although the configuration of your organization's digital certificates will vary based on its specific needs, information about best practices has been included to help you choose the digital certificate configuration that's right for you.

- **Use as few certificates as possible**: Very likely, this means using SAN certificates or wildcard certificates. In terms of interoperability with Exchange, both are functionally equivalent. The decision on whether to use a SAN certificate vs a wildcard certificate is more about the key capabilities or limitations (real or perceived) for each type of certificate as described in the [Digital certificates overview](#digital-certificates-overview).

  For example, if all of your common names will be in the same level of contoso.com, it doesn't matter if you use a SAN certificate or a wildcard certificate. But, if need to use the certificate for autodiscover.contoso.com, autodiscover.fabrikam.com, and autodiscover.northamerica.contoso.com, you need to use a SAN certificate.

- **Use certificates from a commercial CA for client and external server connections**: Although you can configure most clients to trust any certificate or certificate issuer, it's much easier to use a certificate from a commercial CA for client connections to your Exchange servers. No configuration is required on the client to trust a certificate that's issued by a commercial CA. Many commercial CAs offer certificates that are configured specifically for Exchange. You can use the EAC or the Exchange Management Shell to generate certificate requests that work with most commercial CAs.

- **Choose the right commercial CA**: Compare certificate prices and features between CAs. For example:

  - Verify that the CA is trusted by the clients (operating systems, browsers, and mobile devices) that connect to your Exchange servers.

  - Verify that the CA supports the kind of certificate that you need. For example, not all CAs support SAN certificates, the CA might limit the number of common names that you can use in a SAN certificate, or the CA may charge extra based on the number of common names in a SAN certificate.

  - See if the CA offers a grace period during which you can add additional common names to SAN certificates after they're issued without being charged.

  - Verify that the certificate's license allows you to use the certificate on the required number of servers. Some CAs only allow you to use the certificate on one server.

- **Use the Exchange certificate wizard**: A common error when you create certificates is to forget one or more common names that are required for the services that you want to use. The certificate wizard in the Exchange admin center helps you include the correct list of common names in the certificate request. The wizard lets you specify the services that will use the certificate, and includes the common names that you need to have in the certificate for those services. Run the certificate wizard when you've deployed your initial set of Exchange 2016 or Exchange 2019 servers and determined which host names to use for the different services for your deployment.

- **Use as few host names as possible**: Minimizing the number of host names in SAN certificates reduces the complexity that's involved in certificate management. Don't feel obligated to include the host names of individual Exchange servers in SAN certificates if the intended use for the certificate doesn't require it. Typically, you only need to include the DNS names that are presented to the internal clients, external clients, or external servers that use the certificate to connect to Exchange.

  For a simple Exchange Server organization named Contoso, this is a hypothetical example of the minimum host names that would be required:

  - **mail.contoso.com**: This host name covers most connections to Exchange, including Outlook, Outlook on the web, OAB distribution, Exchange Web Services, Exchange admin center, and Exchange ActiveSync.

  - **autodiscover.contoso.com**: This specific host name is required by clients that support Autodiscover, including Outlook, Exchange ActiveSync, and Exchange Web Services clients. For more information, see [Autodiscover service](autodiscover.md).

## Properties of the default self-signed certificates

Some of the more interesting properties of the default self-signed certificates that are visible in the Exchange admin center and/or the Exchange Management Shell on an Exchange server are described in the following table.

||**Microsoft Exchange**|**Microsoft Exchange Server Auth Certificate**|**WMSVC**|
|:-----|:-----|:-----|:-----|
|**Subject**|`CN=<ServerName>` (for example, `CN=Mailbox01`)|`CN=Microsoft Exchange Server Auth Certificate`|`CN=WMSvc-<ServerName>` (for example, `CN=WMSvc-Mailbox01`)|
|**Subject Alternative Names (CertificateDomains)**|• _\<ServerName\>_ (for example, Mailbox01) <br/><br/> • _\<ServerFQDN\>_ (for example, Mailbox01.contoso.com)|none|`WMSvc-<ServerName>` (for example, `WMSvc-Mailbox01`)|
|**Has private key (HasPrivateKey)**|**Yes** (True)|**Yes** (True)|**Yes** (True)|
|**PrivateKeyExportable**<sup>\*</sup>|False|True|True|
|**EnhancedKeyUsageList**<sup>\*</sup>|Server Authentication (1.3.6.1.5.5.7.3.1)|Server Authentication (1.3.6.1.5.5.7.3.1)|Server Authentication (1.3.6.1.5.5.7.3.1)|
|**IISServices**<sup>\*</sup>|`IIS://<ServerName>/W3SVC/1, IIS://<ServerName>/W3SVC/2` (for example, `IIS://Mailbox01/W3SVC/1, IIS://Mailbox01/W3SVC/2`)|none|none|
|**IsSelfSigned**|True|True|True|
|**Issuer**|`CN=<ServerName>` (for example, `CN=Mailbox01`)|`CN=Microsoft Exchange Server Auth Certificate`|`CN=WMSvc-<ServerName>` (for example, `CN=WMSvc-Mailbox01`)|
|**NotBefore**|The date/time that Exchange was installed.|The date/time that Exchange was installed.|The date/time that the IIS Web Manager service was installed.|
|**Expires on (NotAfter)**|5 years after `NotBefore`.|5 years after `NotBefore`.|10 years after `NotBefore`.|
|**Public key size (PublicKeySize)**|2048|2048|2048|
|**RootCAType**|Registry|None|Registry|
|**Services**|IMAP, POP, IIS, SMTP|SMTP|None|

<sup>\*</sup>These properties aren't visible in the standard view in the Exchange Management Shell. To see them, you need to specify the property name (exact name or wildcard match) with the **Format-Table** or **Format-List** cmdlets. For example:

- `Get-ExchangeCertificate -Thumbprint <Thumbprint> | Format-List *`

- `Get-ExchangeCertificate -Thumbprint <Thumbprint> | Format-Table -Auto FriendlyName,*PrivateKey*`

For more information, see [Get-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/get-exchangecertificate).

Further details about the default self-signed certificates that are visible in Windows Certificate Manger are described in the following table.

||**Microsoft Exchange**|**Microsoft Exchange Server Auth Certificate**|**WMSVC**|
|:-----|:-----|:-----|:-----|
|**Signature algorithm**|sha1RSA|sha1RSA|sha1RSA|
|**Signature hash algorithm**|sha1|sha1|sha1|
|**Key usage**|Digital Signature, Key Encipherment (a0)|Digital Signature, Key Encipherment (a0)|Digital Signature, Key Encipherment (a0), Data Encipherment (b0 00 00 00)|
|**Basic constraints**|• `Subject Type=End Entity` <br/>• `Path Length Constraint=None`.|• `Subject Type=End Entity` <br/>• `Path Length Constraint=None`|n/a|
|**Thumbprint algorithm**|sha1|sha1|sha1|

Typically, you don't use Windows Certificate Manger to manage Exchange certificates (use the Exchange admin center or the Exchange Management Shell). Note that the WMSVC certificate isn't an Exchange certificate.
