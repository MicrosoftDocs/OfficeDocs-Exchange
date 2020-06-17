---
title: "Troubleshoot a hybrid deployment"
ms.author: v-mapenn
author: mattpennathe3rd
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
ms.assetid: bbae72f3-6a1e-4cbf-80da-d8f73d969c6b
ms.reviewer:
description: "Configuring a hybrid deployment in Exchange with the Hybrid Configuration wizard greatly minimizes the potential that the hybrid deployment will experience problems. However, there are some typical areas outside the scope of the Hybrid Configuration wizard that, if misconfigured, may present problems in a hybrid deployment. This topic discusses the following common areas where problems may arise and outlines basic steps to verify or correct issues:"
---

# Troubleshoot a hybrid deployment

Configuring a hybrid deployment in Exchange with the Hybrid Configuration wizard greatly minimizes the potential that the hybrid deployment will experience problems. However, there are some typical areas outside the scope of the Hybrid Configuration wizard that, if misconfigured, may present problems in a hybrid deployment. This topic discusses the following common areas where problems may arise and outlines basic steps to verify or correct issues:

- On-premises Exchange servers

- Certificates

- Specific errors of the Hybrid Configuration wizard

> [!NOTE]
> In this topic, "Exchange servers" refers to the following: > **Client Access servers** Exchange 2013 and earlier > **Mailbox servers** Exchange 2016 and later

For additional information, see [Exchange Server Hybrid Deployments](../exchange-hybrid.md).

For additional management tasks related to hybrid deployments, see [Hybrid Deployment procedures](hybrid-deployment.md).

## What do you need to know before you begin?

- Estimated time to complete this task: Varies, depending on type of hybrid deployment issues

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Hybrid deployments" entry in the [Exchange and PowerShell infrastructure permissions](https://docs.microsoft.com/Exchange/permissions/feature-permissions/infrastructure-permissions) topic.

- The guidance in this topic applies to hybrid deployments configured using the Hybrid Configuration wizard. Hybrid deployments that have been manually configured are not supported.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](https://docs.microsoft.com/Exchange/accessibility/keyboard-shortcuts-in-admin-center).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612),[Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## What do you want to do?

### Troubleshoot issues with on-premises Exchange servers

The configuration of the on-premises Exchange servers is typically the area where most problems may occur in a hybrid deployment. Usually, the areas that need to be examined are the following:

- **Availability**: Correctly publishing the on-premises Exchange servers to the Internet is vital to features working correctly in your hybrid deployment. For hybrid features to work correctly, you must configure your on-premises firewall or other security appliances to allow inbound access from the Internet to the Autodiscover and Exchange Web Services (EWS) endpoints on the on-premises Exchange servers. Additionally, the Exchange servers must also be configured to accept inbound SMTP mail. If the Microsoft Exchange Online Protection (EOP) service included in your Microsoft 365 or Office 365 organization can't reach the on-premises Exchange servers, secure mail transport from the Exchange Online organization to the on-premises organization will not function correctly.

- **Certificates**: The digital certificate used for secure mail transport between the on-premises and Exchange Online organizations needs to be installed on all on-premises Internet-facing Exchange servers that will communicate with Exchange Online, must be issued from a third-party certificate authority (CA), must not be expired, and must have the IIS and SMTP services assigned. If these certificate requirements are not met, secure mail transport from the Exchange Online organization to the on-premises organization will not function correctly. More information about certificate requirements is provided in "Troubleshoot issues with Certificates" later in this topic.

#### How do you know if your Exchange servers are configured correctly?

To verify that you have successfully published your on-premises Exchange servers, use the Microsoft Remote Connectivity Analyzer to verify inbound Internet connectivity to your on-premises Exchange servers. Do the following:

1. Go to the [Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/tests/o365) tool.

2. This step is for a general test of EWS tasks to confirm they are working, and that the EWS endpoint is configured.

    Run the **Synchronization, Notification, Availability, and Automatic Replies (OOF)** test in the **Microsoft Exchange Web Services Connectivity Tests** section, and verify that there aren't any errors. If errors occur, correct the items that the test identified.

3. This step is for a general test of the Autodiscover service to confirm that it's working, and that the Autodiscover endpoint is configured.

    Run the **Outlook Autodiscover** test in the **Microsoft Outlook Connectivity Tests** section, and verify that there aren't any errors. If errors occur, correct the items that the test identified.

4. This step is for a general test of SMTP connectivity, and confirms that the Exchange servers can receive inbound Internet mail.

    Run the **Inbound SMTP E-Mail** test in the **Internet E-Mail Tests** section, and verify that there aren't any errors. If errors occur, correct the items that the test identified.

### Troubleshoot issues with certificates

The configuration of the certificates installed on the on-premises Exchange servers may be the source of problems occurring in a hybrid deployment. In most cases, the following certificate-related issues affect hybrid functionality:

- **Certificate type**: The digital certificate used for secure hybrid transport and defined in the Hybrid Configuration wizard must be issued from a third-party CA. Self-signed certificates can't be used for hybrid transport authentication. If a self-signed certificate is inadvertently selected or assigned, secure mail transport between the Exchange Online and the on-premises organizations will not function correctly.

- **Assigned services**: The Internet Information Service (IIS) and the Simple Mail Transport Protocol (SMTP) services must be assigned to the digital certificate used for hybrid transport. If these services aren't assigned, secure mail transport between the Exchange Online and the on-premises organizations will not function correctly.

- **Installation**: The digital certificate used for secure mail transport between the on-premises and Exchange Online organizations must be installed on all on-premises Exchange servers. If you're deploying hybrid with on-premises Edge Transport servers, the digital certificate must also be installed on your Edge Transport servers. If the certificate isn't installed on the on-premises servers, secure mail transport between the Exchange Online and the on-premises organizations will not function correctly.

- **Expiration**: The digital certificate used for secure mail transport between the on-premises and Exchange Online organizations must not be expired. If the certificate is expired, secure mail transport between the Exchange Online and the on-premises organizations will not function correctly.

#### How do you know if your certificates are configured correctly?

To verify that the certificate for hybrid mail transport is correctly configured on your on-premises Exchange servers, do the following:

1. On an on-premises Exchange server, open the Exchange Management Shell.

2. In the Exchange Management Shell, run the following command.

   ```PowerShell
   Get-ExchangeCertificate| format-list
   ```

3. Locate the information for the certificate you defined in the Hybrid Configuration wizard that will be used for secure mail transport.

4. Verify the following parameter values are assigned to the certificate:

   - **IsSelfSigned parameter**: This parameter value should be _False_.

   - **RootCAType parameter**: This parameter value should be _Third Party_.

   - **Services parameter**: This parameter value should be _IIS, SMTP_.

   - **NotAfter parameter**: This parameter value is the certificate expiration date. The date listed here should not be expired.

### Troubleshooting specific errors of the Hybrid Configuration wizard

If you receive an error while running the Hybrid Configuration wizard, you can frequently resolve the issue by performing a few simple checks or actions. See the following suggestions for resolving specific messages or issues that you may encounter while running the Hybrid Configuration wizard.

- **Message: "Default Receive Connector cannot be found on server \<Server Name\>"**: This message appears if the Receive connector on any Exchange server listed in the following attribute isn't listening on TCP port 25 for both the IPv4 and IPv6 protocols: `(Get-HybridConfiguration).ReceivingTransportServers`.

  To verify that the Receive connectors on the Exchange servers listed when you run the `(Get-HybridConfiguration).ReceivingTransportServers.` have the correct bindings, run the following command in the Exchange Management Shell.

  ```PowerShell
  Get-ReceiveConnector -Server <Server Name> | Format-Table Identity, Bindings
  ```

  You should see the following entry listed for your Exchange servers: `{[::]:25, 0.0.0.0:25}`

   If this binding isn't listed, you need to add it to your Receive connector using the _Bindings_ parameter of the **Set-ReceiveConnector** cmdlet. For details, see [Set-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/set-receiveconnector).
