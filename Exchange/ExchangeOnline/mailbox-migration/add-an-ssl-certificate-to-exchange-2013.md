---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 976c080c-fda1-400d-97f4-5b65991cdf4e
ms.reviewer: 
description: Add an SSL certificate to your Exchange server 2013.
title: Add an SSL certificate to Exchange 2013
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- GPA150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Add an SSL certificate to Exchange 2013

Some services, such as Outlook Anywhere, Cutover migration to Office 365, and Exchange ActiveSync, require certificates to be configured on your Exchange 2013 server. This article shows you how to configure an SSL certificate from a third-party certificate authority (CA).

## What permissions do you need?

In order to add certificates, you need to be assigned the [Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) role group on the Exchange Server 2013.

## Tasks for adding an SSL certificate

Adding an SSL certificate to Exchange Server 2013 is a three-step process.

1. Create a certificate request

2. Submit the request to certificate authority

3. Import the certificate

## Create a certificate request

**To create a certificate request**:

1. Open the Exchange admin center (EAC) by browsing to the URL of your Client Access server, for example, https://Ex2013CAS/ECP.

2. Enter your username and password by using the domain\username format for username, and choose **Sign in**.

3. Go to **Servers** \> **Certificates**. On the **Certificates** page, make sure your Client Access server is selected in the **Select server** field, and then choose **New** ![Add icon](media/8ee52980-254b-440b-99a2-18d068de62d3.gif).

4. In the **New Exchange certificate** wizard, select **Create a request for a certificate from a certification authority** and then choose **Next**.

5. Specify a name for this certificate, and then choose **Next**.

6. If you want to request a wildcard certificate, select **Request a wild-card certificate**, and then specify the root domain of all subdomains in the **Root domain** field. If you don't want to request a wildcard certificate and instead want to specify each domain that you want to add to the certificate, leave this page blank. Choose **Next**.

7. Choose **Browse**, and specify an Exchange server to store the certificate on. The server you select should be the internet-facing Client Access server. Choose **Next**.

8. For each service in the list shown, verify that the external or internal server names that users will use to connect to the Exchange server are correct. For example:

   - If you configured your internal and external URLs to be the same, Outlook Web App (when accessed from the internet) and Outlook Web App (when accessed from the intranet) should show owa.contoso.com. Offline Address Book (OAB) (when accessed from the internet) and OAB (when accessed from the intranet) should show mail.contoso.com.

   - If you configured the internal URLs to be internal.contoso.com, Outlook Web App (when accessed from the internet) should show owa.contoso.com, and Outlook Web App (when accessed from the intranet) should show internal.contoso.com.

    These domains will be used to create the SSL certificate request. Choose **Next**.

9. Add any additional domains you want included on the SSL certificate.

10. Select the domain that you want to be the common name for the certificate \> **Set as common name**. For example, contoso.com. Choose **Next**.

11. Provide information about your organization. This information will be included with the SSL certificate. Choose **Next**.

12. Specify the network location where you want this certificate request to be saved. Choose **Finish**.

## Submit the request to certificate authority

After you've saved the certificate request, submit the request to your certificate authority (CA). This can be an internal CA or a third-party CA, depending on your organization. Clients that connect to the Client Access server must trust the CA that you use. You can search the CA website for the specific steps for submitting your request.

## Import the certificate

After you receive the certificate from the CA, complete the following steps.

**To import the certificate request**:

1. On the **Server** \> **Certificates** page in the EAC, select the certificate request you created in the previous steps.

2. In the certificate request details pane, choose **Complete** under **Status**.

3. On the complete pending request page, specify the path to the SSL certificate file \> **OK**.

4. Select the new certificate you just added, and then choose **Edit** ![Edit icon](media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

5. On the certificate page, choose **Services**.

6. Select the services you want to assign to this certificate. At a minimum, you should select SMTP and IIS. Choose **Save**.

7. If you receive the warning **Overwrite the existing default SMTP certificate?**, choose **Yes**.
