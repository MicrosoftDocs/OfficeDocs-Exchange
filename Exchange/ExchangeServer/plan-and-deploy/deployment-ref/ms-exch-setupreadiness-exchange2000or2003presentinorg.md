---
localization_priority: Normal
monikerRange: exchserver-2016 || exchserver-2019
description: Setup can't continue because the organization contains one or more Exchange servers that are too old.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.Exchange2000or2003PresentInOrg
ms.author: v-mapenn
ms.assetid: a115b182-cbd2-4d31-aa0e-375240939301
ms.reviewer: 
title: Can't install Exchange 2016 in a forest that contains Exchange 2000 or Exchange 2003 servers. [Exchange2000or2003PresentInOrg]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Can't install Exchange 2016 in a forest that contains Exchange 2000 or Exchange 2003 servers. [Exchange2000or2003PresentInOrg]

Exchange Setup can't continue because a version of Exchange that's too old for coexistence with the version that you're installing was found in the Active Directory forest. Before you can continue, you need to eliminate all unsupported versions of Exchange from your forest, which might require that you to upgrade to an interim version of Exchange first.

The installation of Exchange Server 2016 or later can't continue because Setup found one or more Exchange 2000 or Exchange 2003 servers in the Active Directory forest. Before you can install Exchange 2016 or later in your organization, you need to remove all Exchange 2000 or Exchange 2003 servers from the forest.

The upgrade path that you need to follow depends on your current version of Exchange. The upgrade paths are described in the following table:

>[!NOTE]
>When you need to upgrade to an interim version of Exchange, you need to migrate all mailboxes, public folders and other components onto the interim version of Exchange before you decommission and remove the earlier Exchange servers.

::: moniker range="exchserver-2019"
|**Current Exchange version**|**Latest Exchange version for coexistence**|**Upgrade path summary**|
|:-----|:-----|:-----|
|Exchange 2000|Exchange 2007|Exchange 2000 \> Exchange 2007 \> Exchange 2013 \> Exchange 2019.|
|Exchange 2003|Exchange 2010|Exchange 2003 \> Exchange 2010 \> Exchange 2016 \> Exchange 2019.|
|Exchange 2007|Exchange 2013|Exchange 2007 \> Exchange 2013 \> Exchange 2019.|
|Exchange 2010|Exchange 2016|Exchange 2010 \> Exchange 2016 \> Exchange 2019.|
::: moniker-end

::: moniker range="exchserver-2016"
|**Current Exchange version**|**Latest Exchange version for coexistence**|**Upgrade path summary**|
|:-----|:-----|:-----|
|Exchange 2000|Exchange 2007|Exchange 2000 \> Exchange 2007 \> Exchange 2013 \> Exchange 2016.|
|Exchange 2003|Exchange 2010|Exchange 2003 \> Exchange 2010 \> Exchange 2016.|
|Exchange 2007|Exchange 2013|Exchange 2007 \> Exchange 2013 \> Exchange 2016.|
|Exchange 2010|Exchange 2016|Exchange 2010 \> Exchange 2016.|
::: moniker-end

When upgrading to Exchange 2013 or later, you can use the Exchange Deployment Assistant to help complete your deployment. For more information, see [Exchange Deployment Assistant](https://assistants.microsoft.com/).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
