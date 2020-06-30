---
title: 'Outlook Anywhere: Exchange 2013 Help'
TOCTitle: Outlook Anywhere
ms:assetid: 9026d461-ec6a-4ef5-ba9d-de33030858f3
ms:mtpsurl: https://technet.microsoft.com/library/Bb123741(v=EXCHG.150)
ms:contentKeyID: 48385337
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Outlook Anywhere

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, the Outlook Anywhere feature, formerly known as RPC over HTTP, lets clients who use Microsoft Outlook 2013, Outlook 2010, or Outlook 2007 connect to their Exchange servers from outside the corporate network or over the Internet using the RPC over HTTP Windows networking component. This topic describes the Outlook Anywhere feature and lists the benefits of using Outlook Anywhere.

## Outlook Anywhere and Exchange 2013

The Windows RPC over HTTP Proxy component, which Outlook Anywhere clients use to connect, wraps remote procedure calls (RPCs) with an HTTP layer. This allows traffic to traverse network firewalls without requiring RPC ports to be opened. In Exchange 2013, this feature is enabled by default, because Exchange 2013 doesn't allow direct RPC connectivity.

## Benefits of using Outlook Anywhere

Outlook Anywhere offers the following benefits to clients that use Outlook 2013, Outlook 2010, or Outlook 2007 to access your Exchange messaging infrastructure:

- Users have remote access to Exchange servers from the Internet.

- You can use the same URL and namespace that you use for Outlook Web App and Microsoft Exchange ActiveSync.

- You can use the same Secure Sockets Layer (SSL) server certificate that you use for both Outlook Web App and Exchange ActiveSync.

- Unauthenticated requests from Outlook can't access Exchange servers.

- You don't have to use a virtual private network (VPN) to access Exchange servers across the Internet.

- If you already use Outlook Web App with SSL or Exchange ActiveSync with SSL, you don't have to open any additional ports from the Internet.

- You can test end-to-end client connectivity for Outlook Anywhere and TCP-based connections by using the **Test-OutlookConnectivity** cmdlet.

## Deploying Outlook Anywhere

In Exchange 2013, Outlook Anywhere is enabled by default, because all Outlook connectivity takes place via Outlook Anywhere. The only post-deployment task you must perform to successfully use Outlook Anywhere is to install a valid SSL certificate on your Client Access server. Mailbox servers in your organization only require the default self-signed SSL certificate.

## Managing Outlook Anywhere

You can manage Outlook Anywhere by using the Exchange admin center or the Exchange Management Shell.

## Outlook Anywhere coexistence

If you are planning to install Exchange 2013 in a coexistence scenario with previous versions of Exchange Server, you might still have Outlook 2003 clients in your organization. Outlook 2003 is not a supported client for Exchange 2013.

Before you move your namespace to Exchange 2013, you need to ensure that all Outlook clients have been upgraded to the minimum supported version. Outlook 2007 or higher is required for an Outlook Anywhere connection to Exchange 2013, even if the target mailbox is still on Exchange 2007 or Exchange 2010.

If you are currently using Outlook Anywhere in your Exchange 2007 or 2010 environments, in order to allow your Exchange 2013 Client Access Server to proxy connections to your Exchange 2007/2010 servers, you must enable and configure Outlook Anywhere on all of the Exchange 2007/2010 CAS in your organization. However, if you are not currently using Outlook Anywhere in your Exchange 2007/2010 environment and you don't want to start using it, then you don't need to enable Outlook Anywhere in the legacy environment. For instructions on enabling Outlook Anywhere for Client Access Servers running on Exchange Server 2007, see [How to Enable Outlook Anywhere](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb123889(v=exchg.80)). For instructions on enabling Outlook Anywhere for Client Access Servers running on Exchange Server 2010, see [Enable Outlook Anywhere](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb123542(v=exchg.141)).

Make sure that when you enable Outlook Anywhere on the Client Access Server, choose NTLM for IIS authentication.

Finally, configure the Outlook Anywhere external host name to point to the Exchange 2013 Outlook Anywhere host name. For instructions for Exchange Server 2007, see [How to Configure an External Host Name for Outlook Anywhere](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa996902(v=exchg.80)). For instructions for Exchange Server 2010, see [Configure an External Host Name for Outlook Anywhere](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/aa996902(v=exchg.141)).

## Testing Outlook Anywhere connectivity

You can test for end-to-end client Outlook connectivity by doing either of the following:

- Running the **Test-OutlookConnectivity** cmdlet. The cmdlet tests for Outlook Anywhere (RPC over HTTP) connections. If the cmdlet test fails, the output notes the step that failed. For detailed syntax and parameters, see [Test-OutlookConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-OutlookConnectivity).

- Running the Outlook Anywhere connectivity test using the Exchange Remote Connectivity Analyzer (ExRCA). When you run this test, you get a detailed summary showing where the test failed and what steps you can take to fix issues. For more information, see [Exchange Remote Connectivity Analyzer](exchange-remote-connectivity-analyzer-exchange-2013-help.md).

Both tests try to sign in through Outlook Anywhere after obtaining server settings from the Autodiscover service. End-to-end verification includes the following:

- Testing for Autodiscover connectivity

- Validating DNS

- Validating certificates (whether the certificate name matches the website, whether the certificate has expired, and whether it's trusted)

- Checking that the firewall is set up correctly (ExRCA checks overall firewall setup. The cmdlet tests for Windows firewall configuration.)

- Confirming client connectivity by signing in to the user's mailbox
