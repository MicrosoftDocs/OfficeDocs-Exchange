---
localization_priority: Normal
description: 'Summary: Learn how to renew Exchange self-signed certificate or create certificate renewal requests for a certification authority in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 356ca7cd-b9d4-487d-aa21-3b38e91bde58
ms.reviewer:
title: Renew an Exchange Server certificate
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Renew an Exchange Server certificate

Every certificate has a built-in expiration date. In Exchange Server, the default self-signed certificate that's installed on the Exchange server expires 5 years after Exchange was installed on the server. You can use the Exchange admin center (EAC) or the Exchange Management Shell to renew Exchange certificates. This includes Exchange self-signed certificates, and certificates that were issued by a certification authority (CA).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For certificates that were issued by a CA, verify the certificate request requirements of the CA. Exchange generates a PKCS #10 request (.req) file that uses Base64 encoding (default) or Distinguished Encoding Rules (DER), with an RSA public key that's 1024, 2048 (default), or 4096 bits. Note that encoding and public key options are only available in the Exchange Management Shell.

- To renew a certificate that was issued by a CA, you need to renew the certificate with the same CA that issued the certificate. If you're changing CAs, or if there's a problem with the original certificate when you try to renew it, you need to create a new certificate request (also known as a certificate signing request or CSR) for a new certificate. For more information, see [Create an Exchange Server certificate request for a certification authority](create-ca-certificate-requests.md).

- If you renew or replace a certificate that was issued by a CA on a subscribed Edge Transport server, you need to remove the old certificate, and then delete and recreate the Edge Subscription. For more information, see [Edge Subscription process](../edge-transport-servers/edge-subscriptions.md#edge-subscription-process).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Renew a certificate that was issued by a certification authority

The procedures are the same for certificates that were issued by an internal CA (for example, Active Directory Certificate Services), or a commercial CA.

To renew a certificate that was issued by a CA, you create a certificate renewal request, and then you send the request to the CA. The CA then sends you the actual certificate file that you need to install on the Exchange server. The procedure is nearly identical to that of completing a new certificate request by installing the certificate on the server. For instructions, see [Complete a pending Exchange Server certificate request](complete-pending-certificate-requests.md).

### Use the EAC to create a certificate renewal request for a certification authority

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server that holds the certificate that you want to renew.

3. All valid certificates have a **Renew** link in the details pane that's visible when you select the certificate from the list. Select the certificate that you want to renew, and then click **Renew** in the details pane.

4. On the **Renew Exchange certificate** page that opens, in the **Save the certificate request to the following file** field, enter the UNC path and filename for the new certificate renewal request file. For example, `\\FileServer01\Data\ContosoCertRenewal.req`. When you're finished, click **OK**.

The certificate request appears in the list of Exchange certificates with a status value of **Pending**.

### Use the Exchange Management Shell to create a certificate renewal request for a certification authority

To create a certificate renewal request for a certification authority on the local Exchange server, use the following syntax:

```PowerShell
Get-ExchangeCertificate -Thumbprint <Thumbprint> | New-ExchangeCertificate -GenerateRequest -RequestFile <FilePathOrUNCPath>\<FileName>.req
```

To find the thumbprint value of the certificate that you want to renew, run the following command:

```PowerShell
Get-ExchangeCertificate | where {$_.Status -eq "Valid" -and $_.IsSelfSigned -eq $false} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,NotBefore,NotAfter
```

This example creates a certificate renewal request with the following properties:

- **Certificate to renew**: `5DB9879E38E36BCB60B761E29794392B23D1C054`

- **RequestFile**: `\\FileServer01\Data\ContosoCertRenewal.req`

```PowerShell
Get-ExchangeCertificate -Thumbprint 5DB9879E38E36BCB60B761E29794392B23D1C054 | New-ExchangeCertificate -GenerateRequest -RequestFile \\FileServer01\Data\ContosoCertRenewal.req
```

 **Notes:**

- The _RequestFile_ parameter accepts a local path or a UNC path.

- We didn't use the _BinaryEncoded_ switch, so the request is Base64 encoded. The information that's displayed onscreen is also written to the file, and the contents of the file are what we need to send to the CA. If we had used the _BinaryEncoded_ switch, the request would have been encoded by DER, and the certificate request file itself is what we would need to send to the CA.

- We didn't use the _KeySize_ parameter, so the certificate request has a 2048 bit RSA public key.

- For more information, see [Get-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/get-exchangecertificate) and [New-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/new-exchangecertificate).

### How do you know this worked?

To verify that you have successfully created a certificate renewal request for a certification authority, perform either of the following steps:

- In the EAC at **Servers** \> **Certificates**, verify the server where you stored the certificate request is selected. The request should be in the list of certificates with the **Status** value **Pending request**.

- In the Exchange Management Shell on the server where you stored the certificate request, run the following command:

  ```PowerShell
  Get-ExchangeCertificate | where {$_.Status -eq "PendingRequest" -and $_.IsSelfSigned -eq $false} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint
  ```

## Renew an Exchange self-signed certificate

When you renew an Exchange self-signed certificate, you're basically making a new certificate.

### Use the EAC to renew an Exchange self-signed certificate

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server that holds the certificate that you want to renew.

3. All valid certificates have a **Renew** link in the details pane that's visible when you select the certificate from the list. Select the certificate that you want to renew, and then click **Renew** in the details pane.

4. On the **Renew Exchange certificate** page that opens, verify the read-only list of Exchange services that the existing certificate is assigned to, and then click **OK**.

### Use the Exchange Management Shell to renew an Exchange self-signed certificate

To renew a self-signed certificate, use the following syntax:

```PowerShell
Get-ExchangeCertificate -Thumbprint <Thumbprint> | New-ExchangeCertificate [-Force] [-PrivateKeyExportable <$true | $false>]
```

To find the thumbprint value of the certificate that you want to renew, run the following command:

```PowerShell
Get-ExchangeCertificate | where {$_.IsSelfSigned -eq $true} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,NotBefore,NotAfter
```

This example renews a self-signed certificate on the local Exchange server, and uses the following settings:

- The thumbprint value of the existing self-signed certificate to renew is `BC37CBE2E59566BFF7D01FEAC9B6517841475F2D`

- The _Force_ switch replaces the original self-signed certificate without a confirmation prompt.

- The private key is exportable. This allows you to export the certificate and import it on other servers.

```PowerShell
Get-ExchangeCertificate -Thumbprint BC37CBE2E59566BFF7D01FEAC9B6517841475F2D | New-ExchangeCertificate -Force -PrivateKeyExportable $true
```

### How do you know this worked?

To verify that you have successfully renewed an Exchange self-signed certificate, use either of the following procedures:

- In the EAC at **Servers** \> **Certificates**, verify the server where you installed the certificate is selected. In the list of certificates, verify that the certificate has **Status** property value **Valid**.

- In the Exchange Management Shell on the server where you renewed the self-signed certificate, run the following command to verify the property values:

```PowerShell
Get-ExchangeCertificate | where {$_.Status -eq "Valid" -and $_.IsSelfSigned -eq $true} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,NotBefore,NotAfter
```
