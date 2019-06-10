---
localization_priority: Normal
description: 'Summary: Learn about how S/MIME in Exchange Server adds S/MIME-based security and lets you encrypt and digitally sign emails.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 887c710b-0ec6-4ff0-8065-5f05f74afef3
ms.date: 7/9/2018
ms.reviewer: 
title: S/MIME for message signing and encryption
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# S/MIME for message signing and encryption

As an administrator in Exchange Server, you can enable Secure/Multipurpose Internet Mail Extensions (S/MIME) for your organization. S/MIME is a widely accepted method (more precisely, a protocol) for sending digitally signed and encrypted messages. S/MIME allows you to encrypt emails and digitally sign them. When you use S/MIME, it helps the people who receive the message by:

- Ensuring that the message in their inbox is the exact message that started with the sender.

- Ensuring that the message came from the specific sender and not from someone pretending to be the sender.

To do this, S/MIME provides for cryptographic security services such as authentication, message integrity, and non-repudiation of origin (using digital signatures). S/MIME also helps enhance privacy and data security (using encryption) for electronic messaging.

S/MIME requires a certificate and publishing infrastructure that is often used in business-to-business and business-to-consumer situations. The user controls the cryptographic keys in S/MIME and can choose whether to use them for each message they send. Email programs such as Outlook search a trusted root certificate authority location to perform digital signing and verification of the signature.

For a more complete background about the history and architecture of S/MIME in the context of email, see [Understanding S/MIME](https://go.microsoft.com/fwlink/p/?LinkID=393948).

## Supported scenarios and technical considerations for S/MIME

You can set up S/MIME to work with any of the following end points:

- Outlook 2010 or later

- Outlook on the web (formerly known as Outlook Web App)

- Exchange ActiveSync (EAS)

The steps that you follow to set up S/MIME with each of these endpoints are slightly different. Generally, you need to complete these steps:

1. Install a Windows-based Certification Authority and set up a public key infrastructure to issue S/MIME certificates. Certificates issued by third-party certificate providers are supported. For details, see [Active Directory Certificate Services Overview](https://technet.microsoft.com/library/hh831740.aspx).

2. Publish the user certificate in an on-premises Active Directory Domain Services (AD DS) account in the **UserSMIMECertificate** and/or **UserCertificate** attributes. Your AD DS needs to be located on computers at a physical location that you control and not at a remote facility or cloud-based service somewhere on the Internet. For more information about AD DS, see [Active Directory Domain Services](https://go.microsoft.com/fwlink/p/?LinkID=394064).

3. Set up a virtual certificate collection in order to validate S/MIME. This information is used by Outlook on the web when validating the signature of an email and ensuring that it was signed by a trusted certificate.

4. Set up the Outlook or EAS end point to use S/MIME.

## Set up S/MIME with Outlook on the web

Setting up S/MIME with Outlook on the web involves these key steps:

1. [Configure S/MIME settings for Outlook on the web](http://technet.microsoft.com/library/c7dee22c-9b5b-425c-91a9-d093204ff84e.aspx)

2. [Set up Virtual Certificate Collection to Validate S/MIME](http://technet.microsoft.com/library/04a616e6-197c-490c-ae8c-c8d5f0f0b3dd.aspx)

For information about how to send an S/MIME encrypted message in Outlook on the web, see [Encrypt messages by using S/MIME in Outlook Web App](https://go.microsoft.com/fwlink/p/?LinkId=392520).

## Related message encryption technologies

A variety of encryption technologies work together to provide protection for messages at rest and in transit. S/MIME can work simultaneously with the following technologies but isn't dependent on them:

> **Transport Layer Security (TLS)**: Encrypts the tunnel or the route between email servers in order to help prevent snooping and eavesdropping, and encrypts the connection between email clients and servers.

**Note**: Secure Sockets Layer (SSL) is being replaced by Transport Layer Security (TLS) as the protocol that's used to encrypt data sent between computer systems. They're so closely related that the terms "SSL" and "TLS" (without versions) are often used interchangeably. Because of this similarity, references to "SSL" in Exchange topics, the Exchange admin center, and the Exchange Management Shell have often been used to encompass both the SSL and TLS protocols. Typically, "SSL" refers to the actual SSL protocol only when a version is also provided (for example, SSL 3.0). To find out why you should disable the SSL protocol and switch to TLS, check out [Protecting you against the SSL 3.0 vulnerability](https://blogs.office.com/2014/10/29/protecting-ssl-3-0-vulnerability/).

> **BitLocker**: Encrypts the data on a hard drive in a datacenter so that if someone gets unauthorized access, they can't read it.
