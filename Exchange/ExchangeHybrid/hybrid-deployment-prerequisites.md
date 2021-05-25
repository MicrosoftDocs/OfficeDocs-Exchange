---
title: "Hybrid deployment prerequisites"
ms.author: dmaguire
author: msdmaguire
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Ent_O365_Hybrid
- Strat_EX_EXOBlocker
- Hybrid
- M365-email-calendar
ms.assetid: e7454db0-fed4-4662-8890-9501126b1ba2
ms.reviewer:
description: "Summary: What your Exchange environment needs before you can set up a hybrid deployment."
---

# Hybrid deployment prerequisites

 **Summary**: What your Exchange environment needs before you can set up a hybrid deployment.

Before you create and configure a hybrid deployment using the Hybrid Configuration wizard, your existing on-premises Exchange organization needs to meet certain requirements. If you don't meet these requirements, you won't be able to complete the steps within the Hybrid Configuration wizard and you won't be able to configure a hybrid deployment between your on-premises Exchange organization and Exchange Online.

## Prerequisites for hybrid deployment

The following prerequisites are required for configuring a hybrid deployment:

- **On-premises Exchange organization**:  The version of Exchange you have installed in your on-premises organization determines the hybrid deployment version you can install. You should typically configure the newest hybrid deployment version that's supported in your organization as described in the following table:

|**On-premises environment**|**Exchange 2019-based hybrid deployment**|**Exchange 2016-based hybrid deployment**|**Exchange 2013-based hybrid deployment**|**Exchange 2010-based hybrid deployment**|
|:-----|:-----|:-----|:-----|:-----|
|Exchange 2019|Supported|Not supported|Not supported|Not supported|
|Exchange 2016|Supported|Supported|Not supported|Not supported|
|Exchange 2013|Supported|Supported|Supported|Not supported|
|Exchange 2010|Not supported|Supported|Supported|Supported|

- **Exchange server releases**: Hybrid deployments require the latest Cumulative Update (CU) or Update Rollup (RU) that's available for your version of Exchange. If you can't install the latest update, the immediately previous release is also supported.

  Exchange CUs are released quarterly, so keeping your Exchange servers up-to-date gives you some additional flexibility if you periodically need extra time to complete upgrades.

- **Exchange server roles**: The server roles you need to install in your on-premises organization depend on the version of Exchange you have installed.

  - **Exchange 2016 and newer**: At least one Mailbox server.

  - **Exchange 2013**: At least one instance of Mailbox and Client Access server roles installed (separately or on one server; we strongly recommend on one server).

  - **Exchange 2010**: At least one instance of Mailbox, Hub Transport, and Client Access server roles installed (separately or on one server; we strongly recommend on one server).

    Hybrid deployments also support Exchange servers running the Edge Transport server role. Edge Transport servers also need to be updated to the latest CU or RU. We strongly recommend that you deploy Edge Transport servers in a perimeter network. You can't deploy Mailbox or Client Access servers in a perimeter network.

> [!NOTE]
  > If you already started a migration process with Exchange 2010 Hybrid endpoints and do not plan to keep on-premises mailboxes, continue your migration as-is. If you plan to keep some mailboxes on-premises, we strongly recommend that you introduce Exchange 2016 Hybrid endpoints (because Exchange 2010 has reached its end of support lifecycle). Continue your migration of Exchange 2010 mailboxes to Office 365, and then move the mailboxes that will stay on-premises to Exchange 2016 servers. After you have removed all of your Exchange 2010 servers, you can then introduce Exchange 2019 servers as your new Hybrid endpoints and also move your remaining on-premises mailboxes to Exchange 2019 servers.
 
- **Microsoft 365 or Office 365**: Hybrid deployments are supported in all Microsoft 365 and Office 365 plans that support Azure Active Directory synchronization. All Microsoft 365 Business Standard, Business Basic, Enterprise, Government, Academic and Midsize plans support hybrid deployments. Microsoft 365 Apps for business and Home plans don't support hybrid deployments.

  Learn more at [Microsoft 365](https://www.microsoft.com/microsoft-365).

- **Custom domains**: Register any custom domains you want to use in your hybrid deployment with Microsoft 365 or Office 365. You can do this by using the Microsoft 365 portal, or by optionally configuring Active Directory Federation Services (AD FS) in your on-premises organization.

  Learn more at [Add your domain to Microsoft 365 or Office 365](/microsoft-365/admin/setup/add-domain).

- **Active Directory synchronization**: Deploy the Azure Active Directory Connect tool to enable Active Directory synchronization with your on-premises organization.

  Learn more at [Azure AD Connect User Sign-on options](/azure/active-directory/hybrid/plan-connect-user-signin).

- **Autodiscover DNS records**: Configure the Autodiscover record for your existing SMTP domains in your public DNS to point to your on-premises Exchange servers (an Exchange 2010/2013 Client Access server or an Exchange 2016/2019 Mailbox Server).

- **Microsoft 365 or Office 365 organization in the Exchange admin center (EAC)**: The Microsoft 365 or Office 365 organization node is available in your on-premises EAC, but you need to use your Microsoft 365 or Office 365 admin credentials to connect the EAC to your Microsoft 365 or Office 365 organization before you can use the Hybrid Configuration wizard. This also allows you to manage both the on-premises and Exchange Online organizations from a single management console.

  Learn more at [Hybrid management in Exchange hybrid deployments](hybrid-management.md).

- **Certificates**: Assign Exchange services to a valid digital certificate that you purchased from a trusted public certificate authority (CA). Although you should use self-signed certificates for the on-premises federation trust with the Microsoft Federation Gateway, you can't use self-signed certificates for Exchange services in a hybrid deployment.

  The Internet Information Services (IIS) instance on the Exchange servers that are configured in the hybrid deployment require a valid digital certificate purchased from a trusted CA.
  
  The EWS external URL and the Autodiscover endpoint that you specified in your public DNS must be listed in the Subject Alternative Name (SAN) field of the certificate. The certificates that you install on the Exchange servers for mail flow in the hybrid deployment must all be issued by the same certificate authority and have the same subject.

  Learn more at [Certificate requirements for hybrid deployments](certificate-requirements.md).

- **EdgeSync**: If you've deployed Edge Transport servers in your on-premises organization and want to configure the Edge Transport servers for hybrid secure mail transport, you need configure EdgeSync prior to using the Hybrid Configuration wizard. You also need to run EdgeSync each time you apply a new CU to an Edge Transport server.

  > [!IMPORTANT]
  > Although EdgeSync is a requirement in deployments with Edge Transport servers, additional configuration settings are required when you configure Edge Transport servers for hybrid secure mail transport.

  Learn more at [Edge Transport servers with hybrid deployments](edge-transport-servers.md).

- **Microsoft .NET Framework**: 4.6.2 or later is required to install Hybrid Configuration Wizard.

- **Unified Messaging-enabled (UM) mailboxes**: If you have UM-enabled mailboxes and you want to move them to Microsoft 365 or Office 365, you need to meet the following requirements **before** you move them:

  - Lync Server 2010, Lync Server 2013, or Skype for Business Server 2015 or later integrated with your on-premises telephony system.
  
    or

  - Skype for Business Online integrated with your on-premises telephony system.
  
    or

  - A traditional on-premises PBX or IP-PBX solution.

    For more information, check out [Telephone system integration with UM in Exchange Online](../ExchangeOnline/voice-mail-unified-messaging/telephone-system-integration-with-um/telephone-system-integration-with-um.md), [Plan for Skype for Business Server and Exchange Server migration](/SkypeForBusiness/hybrid/plan-um-migration), and [Set up Cloud Voicemail](/microsoftteams/set-up-phone-system-voicemail).

## Hybrid deployment protocols, ports, and endpoints

You need to configure the following protocols, ports, and connection endpoints in the firewall that protects your on-premises organization as described in the following table.

  > [!IMPORTANT]
  > The related Microsoft 365 and Office 365 endpoints are vast, ever-changing, and aren't listed here. Instead, see [Microsoft 365 and Office 365 URLs and IP address ranges - Exchange Online](/microsoft-365/enterprise/urls-and-ip-address-ranges#exchange-online) and identify the endpoints for each port listed here.
   
  > [!NOTE]
  > The ports required for mail flow and client connectivity in your on-premises Exchange organization not related to the hybrid configuration are described in [Network ports for clients and mail flow in Exchange](../ExchangeServer/plan-and-deploy/deployment-ref/network-ports.md).
  
|**Source**|**Protocol/Port**|**Target**|**Comments**|
|:-----|:-----|:-----|:-----|
|Exchange Online endpoints|TCP/25 (SMTP/TLS)|Exchange 2019/2016 Mailbox/Edge <br/><br/> Exchange 2013 CAS/Edge <br/><br/> Exchange 2010 Hub/Edge|On-premises Exchange Servers configured to host receive connectors for secure mail transport with Exchange Online in the Hybrid Configuration wizard|
|Exchange 2019/2016 Mailbox/Edge <br/><br/> Exchange 2013 CAS/Edge <br/><br/> Exchange 2010 Hub/Edge|TCP/25 (SMTP/TLS)|Exchange Online endpoints|On-premises Exchange Servers configured to host send connectors for secure mail transport with Exchange Online in the Hybrid Configuration wizard|
|Exchange Online endpoints|TCP/443 (HTTPS)|Exchange 2019/2016 Mailbox <br/><br/> Exchange 2013/2010 CAS|On-premises Exchange Servers used to publish Exchange Web Services and Autodiscover to Internet|
|Exchange 2019/2016 Mailbox <br/><br/> Exchange 2013/2010 CAS|TCP/443 (HTTPS)|Exchange Online endpoints|On-premises Exchange Servers used to publish Exchange Web Services and Autodiscover to Internet|
 
The following table provides more detailed information about the involved on-premises endpoints:

|**Description**|**Port and protocol**|**On-premises endpoint**|**Authentication Provider**|**Authorization Method**|**Pre-Auth Supported?**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|SMTP mail flow between Microsoft 365 or Office 365 and on-premises Exchange|TCP 25 (SMTP/TLS)|Exchange 2019/2016 Mailbox/Edge <br/><br/> Exchange 2013 CAS/Edge <br/><br/> Exchange 2010 Hub/Edge|N/A|Certificate-based|No|
|Autodiscover|TCP 443 (HTTPS)|Exchange 2019/2016 Mailbox server: /autodiscover/autodiscover.svc/wssecurity<br/><br/> Exchange 2013/2010 CAS: /autodiscover/autodiscover.svc|Azure AD authentication system|WS-Security Authentication|No|
|Free/busy, MailTips, and message tracking (EWS)|TCP 443 (HTTPS)|Exchange 2019/2016 Mailbox <br/>or<br/> Exchange 2013/2010 CAS: <br/><br/> /ews/exchange.asmx/wssecurity|Azure AD authentication system|WS-Security Authentication|No|
|Multi-mailbox search (EWS)|TCP 443 (HTTPS)|Exchange 2019/2016 Mailbox <br/>or<br/> Exchange 2013/2010 CAS: <br/><br/> /ews/exchange.asmx/wssecurity <br/> /autodiscover/autodiscover.svc/wssecurity <br/> /autodiscover/autodiscover.svc|Auth Server|WS-Security Authentication|No|
|Mailbox migrations (EWS)|TCP 443 (HTTPS)|Exchange 2019/2016 Mailbox <br/>or<br/> Exchange 2013/2010 CAS: <br/><br/>/ews/mrsproxy.svc|NTLM|Basic|No|
|OAuth (Autodiscover and EWS)|TCP 443 (HTTPS)|Exchange 2019/2016 Mailbox <br/>or<br/> Exchange 2013/2010 CAS: <br/><br/> /ews/exchange.asmx/wssecurity <br/> /autodiscover/autodiscover.svc/wssecurity <br/> /autodiscover/autodiscover.svc|Auth Server|WS-Security Authentication|No|
|AD FS (Windows Server)|TCP 443 (HTTPS)|Windows 2012 R2/2016 Server: /adfs/\*|Azure AD authentication system|Varies per config.|2-factor|
|AAD Connect|TCP 443 (HTTPS)|Windows 2012 R2/2016 Server (AD FS): /adfs/\*|Azure AD authentication system|Varies per config.|2-factor|

For even more detail about this information, see [Deep Dive: How Hybrid Authentication Really Works](https://techcommunity.microsoft.com/t5/exchange-team-blog/deep-dive-how-hybrid-authentication-really-works/ba-p/606780), [Demystifying and troubleshooting hybrid mail flow: when is a message internal?](https://techcommunity.microsoft.com/t5/exchange-team-blog/demystifying-and-troubleshooting-hybrid-mail-flow-when-is-a/ba-p/1420838), [Transport routing in Exchange hybrid deployments](./transport-routing.md), [Configure mail flow using connectors](../ExchangeOnline/mail-flow-best-practices/use-connectors-to-configure-mail-flow/use-connectors-to-configure-mail-flow.md), and [Manage mail flow with mailboxes in multiple locations (Exchange Online and on-premises)](../ExchangeOnline/mail-flow-best-practices/manage-mail-flow-for-multiple-locations.md).

From on-premises Exchange to Office 365, the 2010 MBX & CAS or 2013 MBX (backend), 2016 or 2019 would need outbound Internet access to O365 EWS endpoints as well as the Microsoft Federation Gateway or Authorization server (if using OAuth) via the following URLs:

EWS & Autodiscover:
https://outlook.office365.com/autodiscover/autodiscover.svc
https://autodiscover-s.outlook.com/autodiscover/autodiscover.svc
https://outlook.office365.com/ews/exchange.asmx

MS Federation Gateway:
https://nexus.microsoftonline-p.com/federationmetadata/2006-12/federationmetadata.xml  
https://login.microsoftonline.com/extSTS.srf  
https://domains.live.com/service/managedelegation2.asmx

Authorization Server:
https://login.windows.net/common/oauth2/authorize  
https://accounts.accesscontrol.windows.net/

## Recommended tools and services

The following tools and services are beneficial when you're configuring hybrid deployments with the Hybrid Configuration wizard:

- **Mail migration advisor**: Gives you set-by-step guidance to configure a hybrid deployment between your on-premises organization and Microsoft 365 or Office 365, or migrate completely to Microsoft 365 or Office 365.

  Learn more at [Use the mail migration advisor](mail-migration-jump.md).

- **Remote Connectivity Analyzer tool**: The Microsoft Remote Connectivity Analyzer tool checks the external connectivity of your on-premises Exchange organization and makes sure that you're ready to configure your hybrid deployment. We strongly recommend that you check your on-premises organization with the Remote Connectivity Analyzer tool prior to configuring your hybrid deployment with the Hybrid Configuration wizard.

  Learn more at [Microsoft Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/).

- **Single sign-on**: Single sign-on enables users to access both the on-premises and Exchange Online organizations with a single username and password. It provides users with a familiar sign-on experience and allows administrators to easily control account policies for Exchange Online organization mailboxes by using on-premises Active Directory management tools.

  You have a couple of options when deploying single sign-on: password synchronization and Active Directory Federation Services. Both options are provided by Azure Active Directory Connect. Password synchronization enables almost any organization, no matter the size, to easily implement single sign-on. For this reason, and because the user experience in a hybrid deployment is significantly better with single sign-on enabled, we strongly recommend implement it. For very large organizations, such as those with multiple Active Directory forests that need to join the hybrid deployment, Active Directory Federation Services is required.

  Learn more at [Single sign-on with hybrid deployments](single-sign-on.md).
