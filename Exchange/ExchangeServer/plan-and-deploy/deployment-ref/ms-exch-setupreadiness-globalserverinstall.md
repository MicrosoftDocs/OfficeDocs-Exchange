---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because the user account doesn't have the required permissions.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.GlobalServerInstall
ms.author: chrisda
ms.assetid: 17c4667b-7db1-4e0a-b824-1f6d51d980a9
ms.date: 8/2/2018
ms.reviewer: 
title: Cannot write to the Exchange organization container [GlobalServerInstall]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Cannot write to the Exchange organization container [GlobalServerInstall]

Exchange Setup can't continue because the user account doesn't have the permissions that are required to write to the organization container in the Active Directory directory service.

Setup requires that the account you're using to install Exchange has permissions to create and modify objects in Active Directory:

- If this is the first Exchange server in your organizaiton, your account needs to be a member of the Schema Admins security group (to extend the schema) and the Enterprise Admins security group (to prepare Active Directory).

- After you prepare Active Directory for the version of Exchange that you're installing, your account needs to be a member of the Organization Management role group.

For more information, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md).

To resolve this issue, run Exchange setup again using an account that has the appropriate permissions (grant permissions to the current account or use a different account).


> [!IMPORTANT]
> Cross-forest installation of Exchange isn't supported. Use an account in the Active Directory forest where you're installing Exchange.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
