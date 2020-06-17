---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d4524743-a63f-413f-b290-5f0d2f070392
ms.reviewer: 
description: Steps to add an SSL certificate to Exchange 2007,
title: Add an SSL certificate to Exchange 2007
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- MBS150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Add an SSL certificate to Exchange 2007

Some services, such as Outlook Anywhere, Cutover migration to Office 365, and Exchange ActiveSync, require certificates to be configured on your Microsoft Exchange Server 2007 server. This article shows you how to configure an SSL certificate from a third-party certificate authority (CA).

## Tasks for adding an SSL certificate

Adding an SSL certificate to Microsoft Exchange Server 2007 is a three step process.

1. Create a certificate request

2. Submit the request to certificate authority

3. Import the certificate

## Create a certificate request
<a name="BK_Createrequest"> </a>

To create a certificate request in Microsoft Exchange Server 2007, use the [New-ExchangeCertificate](https://go.microsoft.com/fwlink/p/?LinkId=615756) command. To run the **New-ExchangeCertificate** command, the account you use must be in the Exchange Server Administrator role and local Administrators group for the target server.

 **To create a certificate request**

1. Open the Exchange Management Shell on the local server.

2. On the command line, type:

    ```PowerShell
    New-ExchangeCertificate -DomainName "owa.servername.contoso.com","mail.servername.contoso.com","autodiscover.servername.contoso.com","sts.servername,contoso.com","oos.servername.contoso.com","mail12.servername.contoso.com","edge.servername.contoso.com" -FriendlyName "Exchange 2007 Certificate" -GenerateRequest:$true -KeySize 2048 -Path "C:\certlocation" -PrivateKeyExportable $true -SubjectName "c=us, o=ContosoCorporation, cn=servername,contoso.com"
    ```

    In the command example above, _servername_ is the name of your server, _contoso.com_ is an example of a domain name, and _certlocation_ is a file path to the location where you want to store the request once it is generated. Replace all these placeholders with the information that appropriate for yourMicrosoft Exchange Server 2007.

    In the _DomainName_ parameter, add the domain names for the certificate request. For example, if you configured your internal and external URLs to be the same, the domain name for Outlook Web Access when accessed from the internet or intranet should look like owa._servername_.contoso.com.

    Use the _SubjectName_ parameter to specify the Subject Name on the resulting certificate. This field is used by DNS-aware services and binds a certificate to a particular domain name.

    You must specify the _GenerateRequest_ parameter as `$true`. Otherwise, you will create a self-signed certificate.

3. After you run the above command, a certificate request is saved in the file location you specified by using the _Path_ parameter.

    The **New-ExchangeCertificate** command also creates a _Thumbprint_ output parameter that you use when you submit the request to a third-party certificate authority in the next step.

## Submit the request to certificate authority
<a name="BK_SR"> </a>

After you've saved the certificate request, submit the request to your CA. This can be an internal CA or a third-party CA, depending on your organization. Clients that connect to the Client Access server must trust the CA that you use. You can search the CA website for the specific steps for submitting your request.

## Import the certificate
<a name="BK_import"> </a>

After you receive the certificate from the CA, use the [Import-ExchangeCertificate](https://go.microsoft.com/fwlink/p/?LinkId=615769) command to import it.

 **To import the certificate request**

1. Open the Exchange Management Shell on local server.

2. On the command line, type:

    ```PowerShell
    Import-ExchangeCertificate C:\filepath
    ```

    The _filepath_ parameter above specifies the location where you saved the certificate file that was provided by the third-party CA.

    When you run this command, it creates a _Thumbprint_ output parameter that you use to enable to certificate in the next step.

 **To enable the certificate**

1. To enable the certificate, you use the [Enable-ExchangeCertificate](https://go.microsoft.com/fwlink/p/?LinkId=615770) command. On the command line, type:

    ```PowerShell
    Enable-ExchangeCertificate -Thumbprint 5113ae0233a72fccb75b1d0198628675333d010e -Services iis,smtp,pop,imap
    ```

    The _Thumbprint_ parameter specifies the one you received as output when you ran the **Import-ExchangeCertificate** command.

    In the _Services_ parameter, specify the services you want to assign to this certificate. At a minimum, you should specify SMTP and IIS.

2. If you receive the warning **Overwrite the existing default SMTP certificate?**, type in `A` (yes for all).

## See also
<a name="BK_import"> </a>

[Blog article on adding an SSL to Exchange Server 2007](https://techcommunity.microsoft.com/t5/exchange-team-blog/exchange-2007-autodiscover-and-certificates/ba-p/593753)
