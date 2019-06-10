---
localization_priority: Normal
description: Exchange Server 2016 or Exchange 2019 Setup can't extend the Active Directory schema because the account isn't a member of the Schema Admins and Enterprise Admins security groups.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.SchemaUpdateRequired
ms.author: chrisda
ms.assetid: a4a3f293-afb9-4c00-aa07-c438238b6a98
ms.date: 7/22/2015
ms.reviewer: 
title: The logged-on user is not a member of the Schema Admins group [SchemaUpdateRequired]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# The logged-on user is not a member of the Schema Admins group [SchemaUpdateRequired]

Exchange Setup can't continue because the user account isn't a member of the Schema Admins and Enterprise Admins security groups.

Setup requires that the account you're using to install Exchange has permissions to create and modify objects in Active Directory:

- If this is the first Exchange server in your organizaiton, your account needs to be a member of the Schema Admins security group (to extend the schema) and the Enterprise Admins security group (to prepare Active Directory).

- After you prepare Active Directory for the version of Exchange that you're installing, your account needs to be a member of the Organization Management role group.

For more information, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md).

To resolve this issue, run Exchange setup again using an account that has the appropriate permissions (grant permissions to the current account or use a different account).

> [!IMPORTANT]
> Cross-forest installation of Exchange isn't supported. Use an account in the Active Directory forest where you're installing Exchange.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
