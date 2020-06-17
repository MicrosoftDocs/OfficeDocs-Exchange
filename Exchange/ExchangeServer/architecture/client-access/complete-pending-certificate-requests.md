---
localization_priority: Normal
description: 'Summary: Learn how to complete a pending certificate request in Exchange Server after you receive the certificate from the certification authority.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3d2a8747-4afa-4db8-94d7-dcce6d90d21f
ms.reviewer:
title: Complete a pending Exchange Server certificate request
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Complete a pending Exchange Server certificate request

Completing a pending certificate request (also known as a certificate signing request or CSR) is the next step in configuring Transport Layer Security (TLS) encryption in Exchange Server. After you receive the certificate from the certification authority (CA), you install the certificate on the Exchange server to complete the pending certificate request.

You can complete a pending certificate request in the Exchange admin center (EAC) or in the Exchange Management Shell. The procedures are the same for completing new certificate requests or certificate renewal requests. The procedures are also the same for certificates that were issued by an internal CA (for example, Active Directory Certificate Services), or a commercial CA.

You might receive one or more of the following types of certificate files CA:

- **PKCS #12 certificate files**: These are binary certificate files that have .cer, .crt, .der, .p12, or .pfx filename extensions, and require a password when the file contains the private key or chain of trust. The CA might issue you only one binary certificate file that you need to install (protected by a password), or multiple root or intermediate binary certificate files that you also need to install.

- **PKCS #7 certificate files**: These are text certificate files that have .p7b or .p7c filename extensions. These files contain the text: `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` or `-----BEGIN PKCS7-----` and `-----END PKCS7-----`. If the CA includes a chain of certificates file with your binary certificate file, you also need to install the chain of certificates file.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- The procedures in this topic require you to have created a new certificate request on the Exchange server, sent the certificate request to the CA, and received the certificate from the CA. For more information, see [Create an Exchange Server certificate request for a certification authority](create-ca-certificate-requests.md).

- In the EAC, you need to retrieve the certificate file from a UNC path (`\\<Server>\<Share>` or `\\<LocalServerName>\c$\`). In the Exchange Management Shell, you can use a local file path.

- If you renew or replace a certificate that was issued by a CA on a subscribed Edge Transport server, you need to remove the old certificate, and then delete and recreate the Edge Subscription. For more information, see [Edge Subscription process](../edge-transport-servers/edge-subscriptions.md#edge-subscription-process).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create complete a pending certificate request

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server that holds the pending certificate request.

3. A pending certificate request has the following properties:

   - In the list of certificates, the value of the **Status** field is **Pending request**.

   - When you select the certificate request from the list, there's a **Complete** link in the details pane.

   Select the pending certificate request that you want to complete, and then click **Complete** in the details pane.

4. On the **Complete pending request** page that opens, in the **File to import from** field, enter the UNC path and filename for the certificate file. For example, `\\FileServer01\Data\ContosoCert.cer`. When you're finished, click **OK**.

The certificate request becomes a certificate in the list of Exchange certificates with a **Status** value of **Valid**. For next steps, see the [Next steps](#next-steps) section.

## Use the Exchange Management Shell to complete a pending certificate request

The syntax that you use to complete a pending certificate request in the Exchange Management Shell depends on the type of certificate file or files that you were issued.

To import a binary certificate file (PKCS #12 files that have .cer, .crt, .der, .p12, or .pfx filename extensions), use the following syntax:

```PowerShell
Import-ExchangeCertificate -FileName "<FilePathOrUNCPath>\<FileName>" [-Password (ConvertTo-SecureString -String '<Password> ' -AsPlainText -Force)] [-PrivateKeyExportable <$true | $false>] [-Server <ServerIdentity>]
```

This example imports the binary certificate file `\\FileServer01\Data\Contoso Cert.cer` that's protected by the password P@ssw0rd1 on the local Exchange server.

```PowerShell
Import-ExchangeCertificate -FileName "\\FileServer01\Data\Contoso Cert.cer" -Password (ConvertTo-SecureString -String 'P@ssw0rd1' -AsPlainText -Force)
```

To import a chain of certificates file (PKCS #7 text files that have .p7b or .p7c filename extensions), use the following syntax:

```PowerShell
Import-ExchangeCertificate -FileData ([Byte[]](Get-Content -Encoding Byte -Path "<FilePathOrUNCPath>" -ReadCount 0))]
```

This example imports the text certificate file `\\FileServer01\Data\Chain of Certificates.p7b` on the local Exchange server.

```PowerShell
Import-ExchangeCertificate -FileData "Import-ExchangeCertificate -FileData ([Byte[]](Get-Content -Encoding Byte -Path "\\FileServer01\Data\Chain of Certificates.p7b" -ReadCount 0))]
```

 **Notes:**

- The _FileName_ and _FileData_ parameters accept local paths if the certificate file is located on the Exchange server where you're running the command, and this is the same server where you want to import the certificate. Otherwise, use a UNC path.

- If you want to be able to export the certificate from the server where you're importing it, you need to use the _PrivateKeyExportable_ parameter with the value `$true`.

- For more information, see [Import-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/import-exchangecertificate).

## How do you know this worked?

To verify that you have successfully completed the certificate request and installed the certificate on the Exchange server, use either of the following procedures:

- In the EAC at **Servers** \> **Certificates**, verify the server where you installed the certificate is selected. In the list of certificates, verify that the certificate has **Status** property value **Valid**.

- In the Exchange Management Shell on the server where you installed the certificate, run the following command and verify that the certificate is listed:

  ```PowerShell
  Get-ExchangeCertificate | where {$_.Status -eq "Valid" -and $_.IsSelfSigned -eq $false} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint
  ```

## Next steps

After you complete the pending certificate request by installing the certificate on the server, you need to assign the certificate to one or more Exchange services before the Exchange server is able to use the certificate for encryption. For more information, see [Assign certificates to Exchange services](assign-certificates-to-services.md).
