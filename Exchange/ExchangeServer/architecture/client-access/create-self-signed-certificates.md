---
localization_priority: Normal
description: 'Summary: Learn how to create a new self-signed certificate in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ae826efe-7558-4007-b255-7dfe5933bbbf
ms.reviewer:
title: Create a new Exchange Server self-signed certificate
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a new Exchange Server self-signed certificate

When you install Exchange Server, a self-signed certificate that's created and signed by the Exchange server itself is automatically installed on the server. However, you can also create additional self-signed certificates that you can use.

You can create self-signed certificates certificate in the Exchange admin center (EAC) or in the Exchange Management Shell.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- Exchange self-signed certificates work well for encrypting communication between internal Exchange servers, but not so well for encrypting external connections, because clients, servers, and services don't automatically trust Exchange self-signed certificates. To create a certificate request (also known as a certificate signing request or CSR) for a commercial certification authority that's automatically trusted by all clients, servers, and services, see [Create an Exchange Server certificate request for a certification authority](create-ca-certificate-requests.md).

- When you create a new self-signed certificate by using the **New-ExchangeCertificate** cmdlet, you can assign the certificate to Exchange services during the creation of the certificate. For more information about the Exchange services, see [Assign certificates to Exchange Server services](assign-certificates-to-services.md).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a new Exchange self-signed certificate

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server where you want to install the certificate, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

3. The **New Exchange certificate** wizard opens. On the **This wizard will create a new certificate or a certificate request file** page, select **Create a self-signed certificate**, and then click **Next**.

   **Note:** To create a new certificate request for a certificate authority, see [Create an Exchange Server certificate request for a certification authority](create-ca-certificate-requests.md).

4. On the **Friendly name for this certificate** page, enter a friendly name for the certificate, and then click **Next**.

5. In the **Specify the servers you want to apply this certificate to** page, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png)

   On the **Select a server** page that opens, select the Exchange server where you want to install the certificate, and click **Add - \>**. Repeat this step as many times as necessary. When you're finished selecting servers, click **OK**.

   When you're finished, click **Next**.

6. The **Specify the domains you want to be included in your certificate** page is basically a worksheet that helps you determine the internal and external host names that are required in the certificate for the following Exchange services:

   - Outlook on the web

   - Offline address book generation (OAB)

   - Exchange Web Services

   - Exchange ActiveSync

   - Autodiscover

   - POP

   - IMAP

   - Outlook Anywhere

     If you enter a value for each service based on the location (internal or external), the wizard determines the host names that are required in the certificate, and the information is displayed on the next page. To modify a value for a service, click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)) and enter the host name value that you want to use (or delete the value). When you're finished, click **Next**.

     If you've already determined the host name values that you need in the certificate, you don't need to fill out the information on this page. Instead, click **Next** to manually enter the host names on the next page.

7. The **Based on your selections, the following domains will be included in your certificate** page lists the host names that will be included in the self-signed certificate. The host name that's used in the certificate's **Subject** field is bold, which can be hard to see if that host name is selected. You can verify the host name entries that are required in the certificate based on the selections that you made on the previous page. Or, you can ignore the values from the last page and add, edit, or remove host name values.

   - If you want a SAN certificate, the **Subject** field still requires one common name (CN) value. To select the host name for the certificate's **Subject** field, select the value and click **Set as common name** (check mark). The value should now appear bold.

   - If you want a certificate for a single host name, select the other values one at a time and click **Remove** (![Remove icon](../../media/ITPro_EAC_RemoveIcon.png)).

     When you're finished on this page, click **Finish**.

     **Notes:**

   - You can't delete the bold host name value that will be used for the certificate's **Subject** field. First, you need to select or add a different host name, and then click **Set as common name** (check mark).

   - The changes that you make on this page might be lost if you click the **Back** button.

## Use the Exchange Management Shell to create a new Exchange self-signed certificate

To create a new Exchange self-signed certificate, use the following syntax:

```PowerShell
New-ExchangeCertificate [-FriendlyName <DescriptiveName>] [-SubjectName [C=<CountryOrRegion>,S=<StateOrProvince>,L=<LocalityOrCity>,O=<Organization>,OU=<Department>],CN=<HostNameOrFQDN>]] [-DomainName <Host1>,<Host2>...] [-Services <None | IIS | IMAP | POP | SMTP | UM | UMCallRouter> [-PrivateKeyExportable < $true | $false>] [-Server <ServerIdentity>] -[Force]
```

This example creates a self-signed certificate on the local Exchange server with the following properties:

- **Subject**: _\<ServerName\>_. For example, if you run the command on the server named Mailbox01, the value is `Mailbox01`.

- **Subject alternative names**: _\<ServerName\>_, \<Server FQDN\>. For example, `Mailbox01, Mailbox01.contoso.com`.

- **Friendly name**: Microsoft Exchange

- **Services**: POP, IMAP, SMTP.

```PowerShell
New-ExchangeCertificate
```

This example creates a creates a self-signed certificate on the local Exchange server with the following properties:

- **Subject**: Exchange01, which requires the value `CN=Exchange01`. Note that this value is automatically included in the _DomainName_ parameter (the **Subject Alternative Name** field).

- Additional subject alternative names:

  - mail.contoso.com

  - autodiscover.contoso.com

  - Exchange01.contoso.com

  - Exchange02.contoso.com

- **Services**: SMTP, IIS

- **Friendly name**: Contoso Exchange Certificate

- The private key is exportable. This allows you to export the certificate from the server (and import it on other servers).

```PowerShell
New-ExchangeCertificate -FriendlyName "Contoso Exchange Certificate" -SubjectName CN=Exchange01 -DomainName mail.contoso.com,autodiscover.contoso.com,Exchange01.contoso.com,Exchange02.contoso.com -Services SMTP,IIS -PrivateKeyExportable $true
```

 **Notes:**

- The only required part of the X.500 _SubjectName_ parameter value (the certificate's **Subject** field) is `CN=<HostNameOrFQDN>`.

- Some _Services_ parameter values generate warning or confirmation messages. For more information, see [Assign certificates to Exchange Server services](assign-certificates-to-services.md).

- For more information, see [New-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/new-exchangecertificate).

## How do you know this worked?

To verify that you have successfully created an Exchange self-signed certificate, perform either of the following steps:

- In the EAC at **Servers** \> **Certificates**, verify the server where you created the self-signed certificate is selected. The certificate should be in the list of certificates with the **Status** value **Valid**.

- In the Exchange Management Shell on the server where you created the self-signed certificate, run the following command and verify the properties:

  ```PowerShell
  Get-ExchangeCertificate | where {$_.Status -eq "Valid" -and $_.IsSelfSigned -eq $true} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,NotBefore,NotAfter
  ```
