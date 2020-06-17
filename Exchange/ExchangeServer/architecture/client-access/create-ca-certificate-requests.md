---
localization_priority: Normal
description: 'Summary: Learn how to create a certificate request in Exchange Server that you provide to a certification authority.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: efb00de7-070b-46bf-a2fc-00d07ae085c1
ms.reviewer:
title: Create an Exchange Server certificate request for a certification authority
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create an Exchange Server certificate request for a certification authority

Creating a certificate request is the first step in installing a new certificate on an Exchange server to configure Transport Layer Security (TLS) encryption for one or more Exchange services. You use a certificate request (also known as a certificate signing request or CSR) to obtain a certificate from a certification authority (CA). The procedures are the same for obtaining certificates from an internal CA (for example, Active Directory Certificate Services), or from a commercial CA. After you create the certificate request, you send the results to the CA, and the CA uses the information to issue the actual certificate, which you install later.

You can create certificate requests in the Exchange admin center (EAC) or in the Exchange Management Shell. The **New Exchange certificate** wizard in the EAC can assist you in selecting the host names that are required in the certificate.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes to complete the new certificate request. However, more time is required before the request results in a certificate. For more information, see [Next steps](#next-steps).

- You need to plan carefully to choose the type of certificate that you want, and the host names that are required in the certificate. For more information, see [Digital certificates and encryption in Exchange Server](certificates.md).

- Verify the certificate request requirements of the CA. Exchange generates a PKCS #10 request (.req) file that uses Base64 (default) or Distinguished Encoding Rules (DER) encoding, with an RSA public key that's 1024, 2048 (default), or 4096 bits. Note that encoding and public key options are only available in the Exchange Management Shell.

- In the EAC, you need to store the certificate request file on a UNC path (`\\<Server>\<Share>\` or `\\<LocalServerName>\c$\`). In the Exchange Management Shell, you can specify a local path.

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access services security" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a new certificate request

1. Open the EAC and navigate to **Servers** \> **Certificates**.

2. In the **Select server** list, select the Exchange server where you want to install the certificate, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

3. The **New Exchange certificate** wizard opens. On the **This wizard will create a new certificate or a certificate request file** page, verify that **Create a request for a certificate from a certification authority** is selected, and then click **Next**.

   **Note:** To create a new self-signed certificate, see [Create a new Exchange Server self-signed certificate](create-self-signed-certificates.md).

4. On the **Friendly name for this certificate** page, enter a descriptive name for the certificate, and then click **Next**.

5. On the **Request a wildcard certificate** page, make one of the following choices:

   - **If you want a wildcard certificate**: Select **Request a wildcard certificate**, and enter the wildcard character (\*) and the domain in the **Root domain** field. For example, \*.contoso.com or \*.eu.contoso.com. When you're finished, click **Next**.

   - **If you want a subject alternative name (SAN) certificate**: Make no selections on this page, and click **Next**.

   - **If you want a certificate for a single host**: Make no selections on this page, and click **Next**.

6. In the **Store certificate request on this server** page, click **Browse** and select the Exchange server where you want to store the certificate request (where you want to install the certificate), click **OK**, and then click **Next**.

   **Note:** Steps 7 and 8 only apply to a request for a SAN certificate, or a certificate for a single host. If you selected **Request a wildcard certificate**, skip to Step 9.

7. The **Specify the domains you want to be included in your certificate** page is basically a worksheet that helps you to determine the internal and external host names that are required in the certificate for the following Exchange services:

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

8. The **Based on your selections, the following domains will be included in your certificate** page lists the host names that will be included in the certificate request. The host name that's used in the certificate's **Subject** field is bold, which can be hard to see if that host name is selected. You can verify the host name entries that are required in the certificate based on the selections that you made on the previous page. Or, you can ignore the values from the last page and add, edit, or remove host name values.

   - If you want a SAN certificate, the **Subject** field still requires one common name (CN) value. To select the host name for the certificate's **Subject** field, select the value and click **Set as common name** (check mark). The value should now appear bold.

   - If you want a certificate for a single host name, select the other values one at a time and click **Remove** (![Remove icon](../../media/ITPro_EAC_RemoveIcon.png)).

     **Notes:**

     - You can't delete the bold host name value that will be used for the certificate's **Subject** field. First, you need to select or add a different host name, and then click **Set as common name** (check mark).

     - The changes that you make on this page might be lost if you click the **Back** button.

9. On the **Specify information about your organization** page, enter the following values:

   - **Organization name**

   - **Department name**

   - **City/Locality**

   - **State/Province**

   - **Country/Region name**

     **Note:** These X.500 values are included in the certificate's **Subject** field. Although a value is required in every field before you can proceed, the CA might not care about certain fields (for example, **Department name**), while other fields are very important (for example, **Country/Region name** and **Organization name**). Check the **Subject** field requirements of your CA.

     When you're finished, click **Next**.

10. On the **Save the certificate request to the following file** page, enter the UNC path and filename for the certificate request. For example, `\\FileServer01\Data\ExchCertRequest.req`. When you're finished, click **Finish**.

The certificate request appears in the list of Exchange certificates with a status value of **Pending**. For next steps, see the [Next steps](#next-steps) section.

## Use the Exchange Management Shell to create a new certificate request

To create a new certificate request for a wildcard certificate, a SAN certificate, or a certificate for a single host, use the following syntax:

```PowerShell
New-ExchangeCertificate -GenerateRequest -RequestFile <FilePathOrUNCPath>\<FileName>.req [-FriendlyName <DescriptiveName>] -SubjectName [C=<CountryOrRegion>,S=<StateOrProvince>,L=<LocalityOrCity>,O=<Organization>,OU=<Department>],CN=<HostNameOrFQDN> [-DomainName <Host1>,<Host2>...] [-BinaryEncoded <$true | $false>] [-KeySize <1024 | 2048 | 4096>] [-Server <ServerIdentity>]
```

This example creates a certificate request on the local Exchange server for a wildcard certificate with the following properties:

- **SubjectName**: \*.contoso.com in the United States, which requires the value `C=US,CN=*.contoso.com`.

- **RequestFile**: `\\FileServer01\Data\Contoso Wildcard Cert.req`

- **FriendlyName**: Contoso.com Wildcard Cert

```PowerShell
New-ExchangeCertificate -GenerateRequest -RequestFile "\\FileServer01\Data\Contoso Wildcard Cert.req" -FriendlyName "Contoso.com Wildcard Cert" -SubjectName "C=US,CN=*.contoso.com"
```

This example creates a certificate request on the local Exchange server for a SAN certificate with the following properties:

- **SubjectName**: mail.contoso.com in the United States, which requires the value `C=US,CN=mail.contoso.com`. Note that this CN value is automatically included in the _DomainName_ parameter (the **Subject Alternative Name** field).

- Additional **Subject Alternative Name** field values:

  - autodiscover.contoso.com

  - legacy.contoso.com

  - mail.contoso.net

  - autodiscover.contoso.net

  - legacy.contoso.net

- **RequestFile**: `\\FileServer01\Data\Contoso SAN Cert.req`

- **FriendlyName**: Contoso.com SAN Cert

- **DomainName**: Unquoted comma-separated list of domains

```PowerShell
New-ExchangeCertificate -GenerateRequest -RequestFile "\\FileServer01\Data\Contoso SAN Cert.req" -FriendlyName "Contoso.com SAN Cert" -SubjectName "C=US,CN=mail.contoso.com" -DomainName autodiscover.contoso.com,legacy.contoso.com,mail.contoso.net,autodiscover.contoso.net,legacy.contoso.net
```

This example creates a request for a single subject certificate with the following properties:

- **SubjectName**: mail.contoso.com in the United States, which requires the value `C=US,CN=mail.contoso.com`.

- **RequestFile**: `\\FileServer01\Data\Mail.contoso.com Cert.req`

- **FriendlyName**: Mail.contoso.com Cert

```PowerShell
New-ExchangeCertificate -GenerateRequest -RequestFile "\\FileServer01\Data\Mail.contoso.com Cert.req" -FriendlyName "Mail.contoso.com Cert" -SubjectName "C=US,CN=mail.contoso.com"
```

 **Notes:**

- The only required part of the X.500 _SubjectName_ parameter value (the certificate's **Subject** field) is `CN=<HostNameOrFQDN>`. However, the certificate request should always include the `C=<CountryOrRegion>` value (otherwise, you might not be able to renew the certificate). Check the **Subject** field requirements of your CA.

- The _RequestFile_ parameter accepts a local path or a UNC path.

- We didn't use the _BinaryEncoded_ switch, so the request is Base64 encoded. The information that's displayed onscreen is also written to the file, and the contents of the file are what we need to send to the CA. If we had used the _BinaryEncoded_ switch, the request would have been encoded by DER, and the certificate request file itself is what we would need to send to the CA.

- We didn't use the _KeySize_ parameter, so the certificate request has a 2048 bit RSA public key.

- For more information, see [New-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/new-exchangecertificate).

## How do you know this worked?

To verify that you have successfully created a new certificate request, perform either of the following steps:

- In the EAC at **Servers** \> **Certificates**, verify the server where you stored the certificate request is selected. The request should be in the list of certificates with the **Status** value **Pending request**.

- In the Exchange Management Shell on the server where you stored the certificate request, run the following command:

  ```PowerShell
  Get-ExchangeCertificate | where {$_.Status -eq "PendingRequest" -and $_.IsSelfSigned -eq $false} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint
  ```

## Next steps

The content of a Base64 encoded certificate request file looks like this:

```
-----BEGIN NEW CERTIFICATE REQUEST-----
MIIEBjCCAu4CAQAwYzEWMBQGA1UEAwwNKi5jb250b3NvLmNvbTELMAkGA1UECwwC
SVQxEDAOBgNVBAoMB0NvbnRvc28xEDAOBgNVBAcMB1NlYXR0bGUxCzAJBgNVBAgM
AldBMQswCQYDVQQGEwJVUzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
ANZFK6JxcQMEBitJcEC82vCvr6251o28CMmrpIkl7Z0MnkCrU+BMTLBuZnIgaLvb
jlzORvH6DP/dbyR8gQEAHVrXVWdr3AJIRbqQXWwN++BM5b2O6lIrA8w41XwGNu6r
dtddi+POf8UYwot7PXw6wDsbKaTs1ePVK/0XdemdJCFIXNfCT8LY4p/KryQAyquo
XDa+Acbx7TRxG2kXNAxgPGve+mvyCyizbugXAJIz4nugJ2k/X1kGYDc7f/b80tCv
bPTcGCr09ScsbKmsQcqJ7UxiX2tScpO5AQxNxJHGL+bA6+96FBjPnFZaqPbFgI74
N6hmZdSEDgQlaGfLEGjZBGMCAwEAAaCCAVwwGgYKKwYBBAGCNw0CAzEMFgo2LjEu
NzYwMS4yMEwGCSqGSIb3DQEJDjE/MD0wDgYDVR0PAQH/BAQDAgWgMAwGA1UdEwEB
/wQCMAAwHQYDVR0OBBYEFNRw1o74zcuGyky33rl7WChgdQrlMHIGCisGAQQBgjcN
AgIxZDBiAgEBHloATQBpAGMAcgBvAHMAbwBmAHQAIABSAFMAQQAgAFMAQwBoAGEA
bgBuAGUAbAAgAEMAcgB5AHAAdABvAGcAcgBhAHAAaABpAGMAIABQAHIAbwB2AGkA
ZABlAHIDAQAwfAYJKwYBBAGCNxUUMW8wbQIBBQwrRVhIUi0zMjQ4LkVYSFItMzI0
OGRvbS5leHRlc3QubWljcm9zb2Z0LmNvbQwXRVhIUi0zMjQ4RE9NXEVYSFItMzI0
OCQMIk1pY3Jvc29mdC5FeGNoYW5nZS5TZXJ2aWNlSG9zdC5leGUwDQYJKoZIhvcN
AQEFBQADggEBAL63qVj1m2mBz53+nilnlFweOlcltXoxaF28+Kf0hrJVbH5a2Jme
tS0iKU8YXU3mZ3NnWco+5ea024f9awMIzg4z/heE5yEUFf9UtwRGSOc84r2QexPa
zT/rveTTcbliKU0EFhporl3C2uuBCdAewyLj+/k0hABH3djnmMONG6NyC5f+wMun
kkH5naiSLdsTYbq8jkWYuSqL0qdhtmauqWeAPpA0hKDkQk5eDWpOGx3mgxiaQumo
Rqw6dmQ+o8TC+lE3Tvgdfv47A84X8H7Y9h8liS4h0OfbsgEQb8LcM0YHD6yvPgcD
JCmt8A7JFHF9u6mghjiKlXaZ/i+2l10Wsu8=
-----END NEW CERTIFICATE REQUEST-----
```

You need to send this information to the CA. How you send it depends on the CA, but typically, you send the contents of the file in an email message or in the certificate request form on the CA's web site.

If the CA requires a binary certificate request that's encoded by DER (you used the **New-ExchangeCertificate** cmdlet with the _BinaryEncoded_ switch), you typically send the whole certificate request file to the CA.

After you receive the certificate from the CA, you need to complete the pending certificate request. For instructions, see [Complete a pending Exchange Server certificate request](complete-pending-certificate-requests.md).
