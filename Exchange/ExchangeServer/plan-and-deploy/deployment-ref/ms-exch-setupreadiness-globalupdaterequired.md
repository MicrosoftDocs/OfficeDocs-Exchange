---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because the user account doesn't have the required permissions.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.GlobalUpdateRequired
ms.author: chrisda
ms.assetid: 0530f3c6-6fa6-456b-a33a-f3d2f7eaa2ef
ms.date: 7/22/2015
ms.reviewer: 
title: Global updates required [GlobalUpdateRequired]
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Global updates required [GlobalUpdateRequired]

Exchange Setup can't continue because the user account doesn't have the permissions that are required to write to the organization container in the Active Directory directory service.

Setup requires that the account you're using to install Exchange has permissions to create and modify objects in Active Directory:

- If this is the first Exchange server in your organizaiton, your account needs to be a member of the Schema Admins security group (to extend the schema) and the Enterprise Admins security group (to prepare Active Directory).

- After you prepare Active Directory for the version of Exchange that you're installing, your account needs to be a member of the Organization Management role group.

For more information, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md).

To resolve this issue, run Setup again using an account that has the appropriate permissions (grant permissions to the current account or use a different account).


> [!IMPORTANT]
> Cross-forest installation of Exchange isn't supported. Use an account in the Active Directory forest where you're installing Exchange.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
