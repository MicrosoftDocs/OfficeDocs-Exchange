---
title: 'Assign a certificate to the UM and UM Call Router services: Exchange 2013 Help'
TOCTitle: Assign a certificate to the UM and UM Call Router services
ms:assetid: 8a900e5f-9779-4213-92d7-ec157b15fbc5
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Dn205140(v=EXCHG.150)
ms:contentKeyID: 53908378
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Assign a certificate to the UM and UM Call Router services in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can use the EAC or the Shell to assign a self-signed, internal public key infrastructure (PKI), or third-party commercial certificate for specific Exchange services. When you use the **New-ExchangeCertificate** cmdlet to assign the certificate to Exchange services with the *Services* parameter, you're prompted to assign the certificate to Exchange services. If you use the EAC to create a certificate, the New Exchange Certificate wizard won't prompt you to assign the certificate to Exchange services. You need to edit the properties of the certificate and assign the certificate by selecting which services you want to assign it to.

Different services have different certificate requirements. For example, some services may only require a server name in the **Subject Name** or **Subject Alternative Name** boxes of a certificate and other services may require a fully qualified domain name (FQDN). Make sure that the certificate name can support the uses required by the services you enable it for.

> [!WARNING]
> Self-signed certificates can't be used when you're integrating Unified Messaging (UM) with Microsoft Lync Server.

For additional management tasks related to managing certificates for Unified Messaging, see [Deploying certificates for UM procedures](deploying-certificates-for-um-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Certificate management" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic and the "UM service" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic. You must also log on by using an account that's a member of the local Administrators group on that computer.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to assign a certificate to the Unified Messaging and UM Call Router services

1. In the EAC, navigate to **Servers** \> **Certificates**.

2. In the list view, select the certificate that you want to assign to the Unified Messaging and UM Call Router services, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the *\<Certificate name\>* page, select **Services**, and then select **UM** and **UM call router**.

4. Click **Save**.

## Use the Shell to assign a certificate to the Unified Messaging and UM Call Router services

This example assigns a certificate to the Unified Messaging and UM Call Router services.

```powershell
Enable-ExchangeCertificate -Thumbprint 5113ae0233a72fccb75b1d0198628675333d010e -Services 'UM, UMCallRouter'
```
