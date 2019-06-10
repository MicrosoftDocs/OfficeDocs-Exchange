---
localization_priority: Normal
description: Exchange Server 2016 or Exchange 2019 Setup can't continue because your account isn't logged on to Active Directory.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.LoggedOntoDomain
ms.author: chrisda
ms.assetid: 0e229d10-605a-420f-bf8b-58a7fcb5b259
ms.date: 7/22/2015
ms.reviewer: 
title: The current account isn't logged into an Active Directory domain [LoggedOntoDomain]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# The current account isn't logged into an Active Directory domain [LoggedOntoDomain]

Exchange Setup can't continue because it detected that the current account isn't logged on to an Active Directory domain. You need to log in using an Active Directory account that has the permissions required to install Exchange.

Setup requires that the account you're using to install Exchange has permissions to create and modify objects in Active Directory:

- If this is the first Exchange server in your organizaiton, your account needs to be a member of the Schema Admins security group (to extend the schema) and the Enterprise Admins security group (to prepare Active Directory).

- After you prepare Active Directory for the version of Exchange that you're installing, your account needs to be a member of the Organization Management role group.

For more information, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md).

To resolve this issue, run Setup again using an account that has the appropriate permissions (grant permissions to the current account or use a different account).

> [!IMPORTANT]
> Cross-forest installation of Exchange isn't supported. Use an account in the Active Directory forest where you're installing Exchange.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
