---
title: Import the Exchange Server 2013 Management Pack
TOCTitle: Import the Exchange Server 2013 Management Pack
ms:assetid: dc929928-61b8-448b-9ae5-d3fa73a18ee9
ms:mtpsurl: https://technet.microsoft.com/library/Dn195914(v=EXCHG.150)
ms:contentKeyID: 53181787
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Import the Exchange Server 2013 Management Pack

Let's start with importing the management pack to your SCOM deployment.

_**Applies to:** Exchange Server 2013_

## Prerequisites

Before you can import the management pack, verify that the following conditions are met:

- You have one of the following versions of System Center Operations Manager deployed in your organization:

  - System Center Operations Manager 2012 R2 SP1 or later

- You have already deployed SCOM agents to your Exchange Servers. [Show me how](procedures-related-to-deployment.md).

- The SCOM agents on your Exchange Servers are running under the local system account. [Show me how](procedures-related-to-deployment.md).

- The SCOM agents on your Exchange Servers are configured to act as a proxy and discover managed objects on other computers. [Show me how](procedures-related-to-deployment.md).

- Your user account is a member of the Operations Manager Administrators role.

## Import the Exchange Server 2013 Management Pack

Use the following steps to import the Exchange Server 2013 Management Pack. This procedure assumes that you have extracted the management pack contents to a local drive on your System Center Operations Manager (SCOM) server. You can download the Exchange Server 2013 Management Pack from the.

1. Log on to your SCOM server, and download the Exchange Server 2013 Management Pack from [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39039).

2. Extract the management pack contents to a folder on your system by running the `ExchangeServerManagementPack.msi` file.

3. Start the SCOM Console. In the SCOM console, click **Administration**

4. Right-click **Management Packs**, and then click **Import Management Packs**.

5. The Import Management Packs wizard opens. Click **Add**, and then click **Add from disk**.

6. The **Select Management Packs to import** dialog box appears. Browse to the directory where you extracted the management pack. Click the `Microsoft.Exchange.15.mp` file, and then click **Open**.

7. On the **Select Management Packs** page, the Exchange Server 2013 Management Pack is listed. Click **Install**.

8. The **Import Management Packs** page appears and shows the progress. If there's a problem at any stage of the import process, select the management pack in the list to view the status details.

9. When the import is complete, click **Close**.

10. Click **View** and then **Refresh**, or press F5, to see the Microsoft Exchange Server 2013 management pack in the list of Management Packs.

Now that you have imported the management pack, see [Getting started with Exchange Server 2013 Management Pack](getting-started-with-exchange-server-2013-management-pack.md) to learn about the new dashboard.
