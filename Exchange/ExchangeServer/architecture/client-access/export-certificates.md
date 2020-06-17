---
localization_priority: Normal
description: 'Summary: Learn how to export a certificate from an Exchange server 2016 or 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7e4c4013-8a2b-4c25-a287-b367c65e48aa
ms.reviewer:
title: Export a certificate from an Exchange server
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Export a certificate from an Exchange server

You can export a certificate from an Exchange server as a backup or to import the certificate on other clients, devices or servers. You can export certificates in the Exchange admin center (EAC) or in the Exchange Management Shell. The resulting certificate file is a password-protected binary PKCS #12 file that contains the certificate's private key, and is suitable for importing (installing) on other servers.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- In the EAC, you need to export the certificate file to a UNC path (`\\<Server>\<Share>\` or `\\<LocalServerName>\c$\`). In the Exchange Management Shell, you can specify a local path.

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to export a certificate

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server that contains the certificate, click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png), and select **Export Exchange certificate**.

3. On the **Export Exchange certificate** page that opens, enter the following information:

   - **File to export to**: Enter the UNC path and file name of the certificate file. For example, `\\FileServer01\Data\Fabrikam.pfx`

   - **Password**: When you export the certificate with its private key, you need to specify a password. Exporting the certificate with its private key allows you to import the certificate on other servers.

   When you're finished, click **OK**.

## Use the Exchange Management Shell to export a certificate

To export a binary certificate file that you can import on other clients or servers, use the following syntax:

```PowerShell
Export-ExchangeCertificate -Thumbprint <Thumbprint> -FileName "<FilePathOrUNCPath>\<FileName>.pfx" -BinaryEncoded -Password (ConvertTo-SecureString -String '<Password> ' -AsPlainText -Force) [-Server <ServerIdentity>]
```

This example exports a certificate from the local Exchange server to a file with the following settings:

- The certificate that has the thumbprint value `5113ae0233a72fccb75b1d0198628675333d010e` is exported to the file `C:\Data\Fabrikam.pfx`.

- The exported certificate file is encoded by DER (not Base64).

- The password for the certificate file is `P@ssw0rd1`.

```PowerShell
Export-ExchangeCertificate -Thumbprint 5113ae0233a72fccb75b1d0198628675333d010e -FileName "C:\Data\Fabrikam.pfx" -BinaryEncoded -Password (ConvertTo-SecureString -String 'P@ssw0rd1' -AsPlainText -Force)
```

 **Notes:**

- The _FileName_ parameter accepts a local path or a UNC path.

- You can use a similar procedure to export a pending certificate request (also known as a certificate signing request or CSR). For example, if you need to resubmit the certificate request to the certification authority, and you can't find the original certificate request file. When you export a certificate request, you typically don't need to use the _Password_ parameter or the _BinaryEncoded_ switch, and you save the request to a .req file. Note that you can't import an exported certificate request on another server.

- For more information, see [Export-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/export-exchangecertificate).

## How do you know this worked?

To verify that you have successfully exported a certificate from an Exchange server, try importing the certificate file on another server. For more information, see [Import or install a certificate on an Exchange server](import-certificates.md).
