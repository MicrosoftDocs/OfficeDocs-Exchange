---
localization_priority: Normal
description: 'Summary: Learn how to assign certificates to Exchange services in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f4c170cd-76d3-499d-a34e-8a2bc8724c52
ms.reviewer:
title: Assign certificates to Exchange Server services
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Assign certificates to Exchange Server services

After you install a certificate on an Exchange server, you need to assign the certificate to one or more Exchange services before the Exchange server is able to use the certificate for encryption. You can assign certificates to services in the Exchange admin center (EAC) or in the Exchange Management Shell. Once you assign a certificate to a service, you can't remove the assignment. If you no longer want to use a certificate for a specific service, you need to assign another certificate to the service, and then remove the certificate that you don't want to use.

The available Exchange services are described in the following table.

****

|**Service**|**Uses**|
|:-----|:-----|
|IIS|TLS encryption for internal and external client connections that use HTTP. This includes: <br/> Autodiscover <br/> Exchange ActiveSync <br/> Exchange admin center <br/> Exchange Web Services <br/> Offline address book (OAB) distribution <br/> Outlook Anywhere (RPC over HTTP) <br/> Outlook MAPI over HTTP <br/> Outlook on the web|
|IMAP|TLS encryption for IMAP4 client connections. <br/> Don't assign a wildcard certificate to the IMAP4 service. Instead, use the **Set-ImapSettings** cmdlet to configure the fully qualified domain name (FQDN) that clients use to connect to the IMAP4 service.|
|POP|TLS encryption for POP3 client connections. <br/> Don't assign a wildcard certificate to the POP3 service. Instead, use the **Set-PopSettings** cmdlet to configure the FQDN that clients use to connect to the POP3 service.|
|SMTP|TLS encryption for external SMTP client and server connections. <br/> Mutual TLS authentication between Exchange and other messaging servers. <br/> When you assign a certificate to SMTP, you're prompted to replace the default Exchange self-signed certificate that's used to encrypt SMTP communication between internal Exchange servers. Typically, you don't need to replace the default SMTP certificate.|
|Unified Messaging (UM)|TLS encryption for client connections to the backend UM service on Exchange 2016 Mailbox servers. <br/> You can only assign a certificate to the UM service when the UM startup mode property of the service is set to TLS or Dual. If the UM startup mode is set to the default value TCP, you can't assign the certificate to the UM service. (**Note**: UM is not available in Exchange 2019). For more information, see [Configure the Startup Mode on a Mailbox Server](https://docs.microsoft.com/exchange/configure-the-startup-mode-on-a-mailbox-server-exchange-2013-help).|
|Unified Messaging Call Router (UMCallRouter)|TLS encryption for client connections to the UM Call Router service in the Client Access services on Exchange 2016 Mailbox servers. <br/> You can only assign a certificate to the UM Call Router service when the UM startup mode property of the service is set to TLS or Dual. If the UM startup mode is set to the default value TCP, you can't assign the certificate to the UM Call Router service. (**Note**: UM is not available in Exchange 2019). For more information, see [Configure the Startup Mode on a Client Access Server](https://docs.microsoft.com/exchange/configure-the-startup-mode-on-a-client-access-server-exchange-2013-help).|

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- After you do the procedures in this topic, you might need to restart Internet Information Services (IIS). In some scenarios, Exchange might continue to use the previous certificate for encrypting and decrypting the cookie that's used for Outlook on the web (formerly known as Outlook Web App) authentication. We recommend restarting IIS in environments that use Layer 4 load balancing.

- If you renew or replace a certificate that was issued by a CA on a subscribed Edge Transport server, you need to remove the old certificate, and then delete and recreate the Edge Subscription. For more information, see [Edge Subscription process](../edge-transport-servers/edge-subscriptions.md#edge-subscription-process).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to assign a certificate to Exchange services

1. Open the EAC, and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server that holds the certificate.

3. Select the certificate that you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). The certificate needs to have the **Status** value **Valid**.

4. On the **Services** tab, in the **Specify the services you want to assign this certificate to** section, select the services. Remember, you can add services, but you can't remove them. When you're finished, click **Save**.

## Use the Exchange Management Shell to assign a certificate to Exchange services

To assign a certificate to Exchange services, use the following syntax:

```PowerShell
Enable-ExchangeCertificate -Thumbprint <Thumbprint> -Services <Service1>,<Service2>... [-Server <ServerIdentity>]
```

This example assigns the certificate that has the thumbprint value `434AC224C8459924B26521298CE8834C514856AB` to the POP, IMAP, IIS, and SMTP services.

```PowerShell
Enable-ExchangeCertificate -Thumbprint 434AC224C8459924B26521298CE8834C514856AB -Services POP,IMAP,IIS,SMTP
```

You can find the certificate thumbprint value by using the **Get-ExchangeCertificate** cmdlet.

## How do you know this worked?

To verify that you have successfully assigned a certificate to one or more Exchange services, use either of the following procedures:

- In the EAC at **Servers** \> **Certificates**, verify the server where you installed the certificate is selected. Select the certificate, and in the details pane, verify that the **Assigned to services property** contains the services that you selected.

- In the Exchange Management Shell on the server where you installed the certificate, run the following command to verify the Exchange services for the certificate:

  ```PowerShell
  Get-ExchangeCertificate | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,Services
  ```
