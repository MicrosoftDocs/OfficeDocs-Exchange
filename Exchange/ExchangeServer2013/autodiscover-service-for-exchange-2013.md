---
title: Autodiscover service for Exchange 2013
TOCTitle: Autodiscover service
ms:assetid: b03c0f21-cbc2-4be8-ad03-73a7dac16ffc
ms:mtpsurl: https://technet.microsoft.com/library/Bb124251(v=EXCHG.150)
ms:contentKeyID: 50396327
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Autodiscover service

_**Applies to:** Exchange Server 2013_

Learn about the Exchange Autodiscover service for Microsoft Exchange 2013. You'll learn what the Exchange Autodiscover service does and how it works as well as what the deployment options are.

Microsoft Exchange 2013 includes a service named the Autodiscover service. This topic gives an overview of the service and explains how it works, how it configures Outlook clients, and what options there are for deploying the Autodiscover service in your messaging environment.

The Autodiscover service does the following:

- Automatically configures user profile settings for clients running Microsoft Office Outlook 2007, Outlook 2010, or Outlook 2013, as well as supported mobile phones. Phones running Windows Mobile 6.1 or a later version are supported. If your phone isn't a Windows Mobile phone, check your mobile phone documentation to see if it's supported.

- Provides access to Exchange features for Outlook 2007, Outlook 2010, or Outlook 2013 clients that are connected to your Exchange messaging environment.

- Uses a user's email address and password to provide profile settings to Outlook 2007, Outlook 2010, or Outlook 2013 clients and supported mobile phones. If the Outlook client is joined to a domain, the user's domain account is used.

## Overview of the Autodiscover service

The Autodiscover service makes it easier to configure Outlook 2007, Outlook 2010, or Outlook 2013 and some mobile phones. You can't use the Autodiscover service with versions of Outlook earlier than Office Outlook 2007. In earlier versions of Microsoft Exchange (Exchange 2003 SP2 or earlier) and Outlook (Outlook 2003 or earlier), you had to configure all user profiles manually to access Exchange. Extra work was required to manage these profiles if changes occurred to the messaging environment. Otherwise, the Outlook clients would stop functioning correctly.

Through the Autodiscover service, Outlook finds a new connection point made up of the user's mailbox GUID + @ + the domain portion of the user's primary SMTP address. The Autodiscover service returns the following information to the client:

- The user's display name

- Separate connection settings for internal and external connectivity

- The location of the user's Mailbox server

- The URLs for various Outlook features that govern functionality such as free/busy information, Unified Messaging, and the offline address book

- Outlook Anywhere server settings

When a user's Exchange information is changed, Outlook automatically reconfigures the user's profile using the Autodiscover service. For example, if a user's mailbox is moved or the client can't connect to the user's mailbox or to available Exchange features, Outlook will contact the Autodiscover service and automatically update the user's profile to include the information that's required to connect to the mailbox and Exchange features.

## How the Autodiscover service works

When you install a Client Access server in Exchange 2013, a default virtual directory named Autodiscover is created under the default website in Internet Information Services (IIS). This virtual directory handles Autodiscover service requests from Outlook 2007, Outlook 2010, and Outlook 2013 clients and supported mobile phones under the following circumstances:

- When a user account is configured or updated

- When an Outlook client periodically checks for changes to the Exchange Web Services URLs

- When underlying network connection changes occur in your Exchange messaging environment

Additionally, a new Active Directory object named the service connection point (SCP) is created on the server where you install the Client Access server.

The SCP object contains the authoritative list of Autodiscover service URLs for the forest. You can use the **Set-ClientAccessServer** cmdlet to update the SCP object. For more information, see [Set-ClientAccessServer](https://docs.microsoft.com/powershell/module/exchange/Set-ClientAccessServer).

> [!IMPORTANT]
> Before you run the <STRONG>Set-ClientAccessServer</STRONG> cmdlet, make sure the Authenticated Users account on the Client Access server has Read permissions for the SCP object. If users don't have the correct permissions, they can't search for and read items.

For more information about SCP objects, see [Publishing with Service Connection Points](https://docs.microsoft.com/windows/win32/ad/publishing-with-service-connection-points).

For external access, or using DNS, the client locates the Autodiscover service on the Internet by using the primary SMTP domain address from the user's email address.

> [!NOTE]
> You must provide a record in DNS for Outlook clients to discover the Autodiscover service by using DNS. For more information, see your Windows documentation for configuring DNS and also see the <A href="https://go.microsoft.com/fwlink/p/?linkid=85214">White Paper: Exchange 2007 Autodiscover Service</A>.

Depending on whether you've configured the Autodiscover service on a separate site, the Autodiscover service URL will be either https://\<*smtp-address-domain*\>/autodiscover/autodiscover.xml or https://autodiscover.\<*smtp-address-domain*\>/autodiscover/autodiscover.xml, where ://\<*smtp-address-domain*\> is the primary SMTP domain address. For example, if the user's email address is tony@contoso.com, the primary SMTP domain address is contoso.com. When the client connects to Active Directory, the client looks for the SCP object created during Setup. In deployments that include multiple Client Access servers, an Autodiscover SCP object is created for each Client Access server. The SCP object contains the *ServiceBindingInfo* attribute with the fully qualified domain name (FQDN) of the Client Access server in the form https://CAS01/autodiscover/autodiscover.xml, where CAS01 is the FQDN for the Client Access server. Using the user credentials, the Outlook 2007, Outlook 2010, or Outlook 2013 client authenticates to Active Directory and searches for the Autodiscover SCP objects. After the client obtains and enumerates the instances of the Autodiscover service, the client connects to the first Client Access server in the enumerated list and obtains the profile information in the form of XML data that's needed to connect to the user's mailbox and available Exchange features.

## Deployment options for the Autodiscover service

The Autodiscover service must be deployed and configured correctly for Outlook 2007, Outlook 2010, and Outlook 2013 clients to automatically connect to Exchange features such as the offline address book, the Availability service, and Unified Messaging (UM). Deploying the Autodiscover service is only one step in making sure your Exchange services, such as the Availability service, can be accessed by Outlook 2007, Outlook 2010, or Outlook 2013 clients.

## Configuring Autodiscover for cross-forest moves

The Autodiscover service can provide user profile information to connecting Outlook clients for mailboxes that have been moved from one Exchange forest to another. For this to happen, you must configure a mail-enabled user in both the original forest where the user's mailbox resided and in the target forest using the **New-MailUser** cmdlet. In the source forest, you should use the *ExternalEmailAddress* parameter in the cmdlet to specify the new email address of the mailbox in the target forest. For more information, see [New-MailUser](https://docs.microsoft.com/powershell/module/exchange/New-MailUser).

When you configure a mail-enabled user, the Autodiscover service in the original forest will redirect the authenticating user to the new email address in the target forest. The connecting Outlook client will then be redirected to the Client Access server in the target forest where the mailbox has been moved.
