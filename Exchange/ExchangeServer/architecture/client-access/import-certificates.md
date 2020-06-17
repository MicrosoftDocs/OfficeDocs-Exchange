---
localization_priority: Normal
description: 'Summary: Learn how to import (install) a certificate on Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 43fbe354-ccfc-45b0-9fbc-4b23c6c5ccf4
ms.reviewer:
title: Import or install a certificate on an Exchange server
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Import or install a certificate on an Exchange server

To enable encryption for one or more Exchange services, the Exchange server needs to use a certificate. SMTP communication between internal Exchange servers is encrypted by the default self-signed certificate that's installed on the Exchange server. To encrypt communication with internal or external clients, servers, or services, you'll likely want to use a certificate that's automatically trusted by all clients, services and servers that connect to your Exchange organization. For more information, see [Certificate requirements for Exchange services](certificates.md#certificate-requirements-for-exchange-services).

You can import (install) certificates on Exchange servers in the Exchange admin center (EAC) or in the Exchange Management Shell.

These are the types of certificate files that you can import on an Exchange server:

- **PKCS #12 certificate files**: These are binary certificate files that have .cer, .crt, .der, .p12, or .pfx filename extensions, and require a password when the file contains the private key or chain of trust. Examples of these types of files include:

  - Self-signed certificates that were exported from other Exchange servers by using the EAC or the **Export-ExchangeCertificate** with the _PrivateKeyExportable_ parameter value `$true`. For more information, see [Export a certificate from an Exchange server](export-certificates.md).

  - Certificates that were issued by a certification authority (an internal CA like Active Directory Certificate Services, or a commercial CA).

  - Certificates that were exported from other servers (for example, Skype for Business Server).

- **PKCS #7 certificate files**: These are text certificate files that have .p7b or .p7c filename extensions. These files contain the text: `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` or `-----BEGIN PKCS7-----` and `-----END PKCS7-----`. A certificate authority might include a chain of certificates file that also needs to be installed along with the actual binary certificate file.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- In the EAC, you need to import the certificate file from a UNC path (`\\<Server>\<Share>\` or `\\<LocalServerName>\c$\`). In the Exchange Management Shell, you can specify a local path.

- In the EAC, you can import the certificate file on multiple Exchange servers at the same time (Step 4 in the procedure).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to import a certificate on one or more Exchange servers

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server where you want to install the certificate, click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png), and select **Import Exchange certificate**.

3. The **Import Exchange certificate** wizard opens. On the **This wizard will import a certificate from a file** page, enter the following information:

   - **File to import from**: Enter the UNC path and filename of the certificate file. For example, `\\FileServer01\Data\Fabrikam.cer`

   - **Password**: If the certificate file contains the private key or chain of trust, the file is protected by a password. Enter the password here.

   When you're finished, click **Next**.

4. In the **Specify the servers you want to apply this certificate to** page, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png)

   On the **Select a server** page that opens, select the Exchange server where you want to install the certificate, and click **Add - \>**. Repeat this step as many times as necessary. When you're finished selecting servers, click **OK**.

   When you're finished, click **Finish**. For next steps, see the [Next steps](#next-steps) section.

## Use the Exchange Management Shell to import a certificate on an Exchange server

To import a binary certificate file (PKCS #12 files that have .cer, .crt, .der, .p12, or .pfx filename extensions), use the following syntax:

```PowerShell
Import-ExchangeCertificate -FileName "<FilePathOrUNCPath>\<FileName>" -Password (ConvertTo-SecureString -String '<Password> ' -AsPlainText -Force) [-PrivateKeyExportable <$true | $false>] [-Server <ServerIdentity>]
```

This example imports the certificate file `\\FileServer01\Data\Fabrikam.pfx` that's protected by the password P@ssw0rd1 on the local Exchange server.

```PowerShell
Import-ExchangeCertificate -FileName "\\FileServer01\Data\Fabrikam.pfx" -Password (ConvertTo-SecureString -String 'P@ssw0rd1' -AsPlainText -Force)
```

To import a chain of certificates file (PKCS #7 text files that have .p7b or .p7c filename extensions) that's associated with a certificate, use the following syntax:

```PowerShell
Import-ExchangeCertificate -FileData ([Byte[]](Get-Content -Encoding Byte -Path "<FilePathOrUNCPath>" -ReadCount 0))
```

This example imports the chain of certificates file `\\FileServer01\Data\Chain of Certificates.p7b`.

```PowerShell
Import-ExchangeCertificate -FileData ([Byte[]](Get-Content -Encoding Byte -Path "\\FileServer01\Data\Chain of Certificates.p7b" -ReadCount 0))
```

 **Notes:**

- You need to repeat this procedure on each Exchange server where you want to import the certificate (run the command on the server, or use the _Server_ parameter).

- The _FileName_ and _FileData_ parameters accept local paths if the certificate file is located on the Exchange server where you're running the command, and this is the same server where you want to import the certificate. Otherwise, use a UNC path.

- If you want to be able to export the certificate from the server where you're importing it, you need to use the _PrivateKeyExportable_ parameter with the value `$true`.

- For more information, see [Import-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/import-exchangecertificate).

## How do you know this worked?

To verify that you have successfully imported (installed) a certificate on an Exchange server, use either of the following procedures:

- In the EAC at **Servers** \> **Certificates**, verify the server where you installed the certificate is selected. The certificate should be in the list of certificates with the **Status** value **Valid**.

- In the Exchange Management Shell on the server where you installed the certificate, run the following command:

  ```PowerShell
  Get-ExchangeCertificate | where {$_.Status -eq "Valid"} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,NotBefore,NotAfter
  ```

## Next steps

After you install the certificate on the server, you need to assign the certificate to one or more Exchange services before the Exchange server is able to use the certificate for encryption. For more information, see [Assign certificates to Exchange Server services](assign-certificates-to-services.md).
