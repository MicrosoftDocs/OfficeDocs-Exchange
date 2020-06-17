---
title: "Certificate requirements for hybrid deployments"
ms.author: v-mapenn
author: mattpennathe3rd
ms.reviewer:
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: 48d532cc-29f9-4009-9d2d-f19a9c13c320
description: "In a hybrid deployment, digital certificates are an important part of securing the communication between the on-premises Exchange organization and Microsoft 365 and Office 365. Certificates enable each Exchange organization to trust the identity of another. Certificates also help to ensure that each Exchange organization is communicating to the right source."
---

# Certificate requirements for hybrid deployments

In a hybrid deployment, digital certificates are an important part of securing the communication between the on-premises Exchange organization and Microsoft 365 or Office 365. Certificates enable each Exchange organization to trust the identity of another. Certificates also help to ensure that each Exchange organization is communicating to the right source.

In a hybrid deployment, many services make use of certificates:

- **Azure Active Directory Connect (Azure AD Connect) with Active Directory Federation Services (AD FS)**: If you choose to deploy Azure AD Connect with AD FS as part of your hybrid deployment, a certificate issued by a trusted third-party certificate authority (CA) is used to establish a trust between web clients and federation server proxies, to sign security tokens, and to decrypt security tokens.

    Learn more at [Certificates](https://go.microsoft.com/fwlink/p/?linkId=205993).

- **Exchange federation**: A self-signed certificate is used to create a secure connection between the on-premises Exchange servers and the Azure Active Directory authentication system.

    Learn more at [Sharing](https://docs.microsoft.com/exchange/sharing-exchange-2013-help).

- **Exchange services**: Certificates issued by a trusted third-party CA are used to help secure Secure Sockets Layer (SSL) communication between Exchange servers and clients. Services that use certificates include Outlook on the web, Exchange ActiveSync, Outlook Anywhere, and secure message transport.

- **Existing Exchange servers**: Your existing Exchange servers may make use of certificates to help secure Outlook on the web communication, message transport, and so on. Depending on how you use certificates on your Exchange servers, you might use self-signed certificates or certificates issued by a trusted third-party CA.

## Certificate requirements for a hybrid deployment

When configuring a hybrid deployment, you must use and configure certificates that you have purchased from a trusted third-party CA. The certificate used for hybrid secure mail transport must be installed on all on-premises Mailbox (Exchange 2016 and newer), and Mailbox and Client Access (Exchange 2013 and older) servers.

> [!IMPORTANT]
> If you're configuring a hybrid deployment in an organization that has Exchange servers deployed in multiple Active Directory forests, you must use a separate third-party CA certificate for each Active Directory forest. <br/><br/> When Exchange Edge Transport servers are deployed in an on-premises organization, this certificate must also be installed on all Edge Transport servers. Each Edge transport server must use a certificate that shares the same issuing CA and the same subject for hybrid secure mail to function correctly.

Multiple services, such as AD FS, Exchange federation, services, and Exchange, each require certificates. Depending on your organization, you may decide to do one of the following:

- Use a third-party certificate that's used by all services across multiple servers.

- Use a third-party certificate for each server that provides services.

Whether you choose to use the same certificate for all services or dedicate a certificate for each service depends on your organization and the service you're implementing. Here are some things to consider about each option:

- **Third-party certificate across multiple servers**: Third-party certificates that are used by services across multiple servers may be slightly cheaper to obtain, but they may complicate renewal and replacement. The complication occurs because, when a certificate needs replacement, you need to replace the certificate on every server where it's installed.

- **Third-party certificate for each server**: Using a dedicated certificate for each server that hosts services allows you to configure the certificate specifically for the services on that server. If you need to replace the certificate or renew it, you only need to replace it on the server where the services are installed. Other servers aren't impacted.

We recommend that you use a dedicated third-party certificate for any optional AD FS server, another certificate for the Exchange services for your hybrid deployment, and if needed, another certificate on your Exchange servers for other needed services or features. The on-premises federation trust configured as part of federated sharing in a hybrid deployment uses a self-signed certificate by default. Unless you have specific requirements, there's no need to use a third-party certificate with the federation trust configured as part of a hybrid deployment.

The services that are installed on a single server may require that you configure multiple fully qualified domain names (FQDNs) for the server. You should purchase a certificate that allows for the maximum required number of FQDNs. Certificates consist of the subject (also called a principal name) and one or more subject alternative names (SAN). The subject name is the FQDN that the certificate is issued to and should use the primary SMTP domain that is shared between the on-premises and Exchange Online organizations. SANs are additional FQDNs that can be added to a certificate in addition to the subject name. If you need a certificate to support five FQDNs, purchase a certificate that allows for five domains to be added to the certificate: one subject name and four SANs.

The following table outlines the minimum suggested FQDNs that should be included on certificates configured for use in a hybrid deployment.

|**Service**|**Suggested FQDN**|**Field**|
|:-----|:-----|:-----|
|Primary shared SMTP domain|contoso.com|Subject name|
|Autodiscover|Label that matches the external Autodiscover FQDN of your Exchange 2013 Client Access server, such as autodiscover.contoso.com|Subject alternative name|
|Transport|Label that matches the external FQDN of your Edge Transport servers, such as edge.contoso.com|Subject alternative name|
