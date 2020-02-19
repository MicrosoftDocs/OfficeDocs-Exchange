---
title: "About the Office 365 Best Practices Analyzer for Exchange Server 2013"
ms.author: v-mapenn
author: mattpennathe3rd
ms.reviewer: 
manager: serdars
audience: Admin
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Adm_O365
- Hybrid
- M365-email-calendar
f1.keywords:
- CSH
ms.custom:
- Adm_O365
- httpsfix
- MiniMaven
search.appverid:
- MET150
- MOE150
ms.assetid: c137e46e-c05d-4bdc-b968-67fe5ae68765
description: "Admins can learn about the requirements and capabilities of the BPA for Exchange Server"
---

# About the Office 365 Best Practices Analyzer for Exchange Server 2013

The Office 365 Best Practices Analyzer is an automated tool that evaluates the health and readiness of your on-premises Exchange environment. You can run the Office 365 Best Practices Analyzer on Exchange Server 2013 at any time to assess the state of your Exchange configuration.

## Overview

You can use the Office 365 Best Practices Analyzer in the following environments:

- Exchange 2013 (not supported in Exchange Server 2016 or Exchange Server 2019)

- Hybrid configuration with Exchange 2013 (not supported with Exchange 2016 or Exchange 2019)

You'll need an Office 365 or Azure Active Directory account to install and use the tool. After you've installed and run the tool, you won't be required to sign-in to the Microsoft 365 admin center to re-run the checks (although you might be prompted to sign-in again).

## Prerequisites

To run the the Office 365 Best Practices Analyzer on your Exchange server, you'll need to meet the following requirements. We'll automatically verify that you're ready to run the checks when you download the tool.

- Exchange 2013

- Internet Explorer 9.0 or later

- The Windows Management Framework (WMF) version 3.0 or later (includes Windows PowerShell and Windows Remote Management or WinRM). Windows Server 2012 or later already has the required version of the WMF. Note that installing a separate, downloadable version of the WMF isn't supported on Exchange 2013 or later servers.

- The 64-bit versions of the Microsoft Azure Active Directory Module for Windows PowerShell and the Microsoft Online Services Sign-in Assistant. You can install them as described [here](https://docs.microsoft.com/office365/enterprise/powershell/connect-to-office-365-powershell#step-1-install-required-software-1).

- A minimum screen resolution of 1024 x 768 (XGA).

## How to use the Office 365 Best Practices Analyzer

|**Task**|**Actions**|
|:-----|:-----|
|Step 1: Log on and sign in.|Remote Desktop into your on-premises Exchange server and open the Exchange admin center (EAC). For example, https://localhost/ecp. <br/><br/> If you're an Office 365 Enterprise or Office 365 Midsize Business admin, open Internet Explorer and browse to the [About the admin center](https://docs.microsoft.com/office365/admin/admin-overview/about-the-admin-center) with your Office 365 account. <br/><br/> If you don't have Office 365 yet, you can sign up [here](https://products.office.com/business/compare-more-office-365-for-business-plans).|
|Step 2: Install the Best Practices Analyzer.|1. In the EAC, go to **Tools** \> **Checks** and click **Check your on-premises Exchange Server with the Office 365 Best Practices Analyzer**. <br/>2. Click **Run** when you're prompted to download or run the tool. <br/>3. Click **Accept** on the end user license agreements for each of the required prerequisites (the Microsoft Online Services Sign-in Assistant, the .NET Framework, and the Windows Azure Active Directory Module for Windows PowerShell). <br/>4. In the **Application Install - Security Warning** dialog, click Install. <br/>5. In the **Terms of Use for the Office 365 Best Practices Analyzer**, click **Accept**.|
|Step 3: Run the Best Practices Analyzer.|After the Office 365 Best Practices Analyzer is installed for the first time, it should start automatically. You can also run it later by selecting **Microsoft Office 365 Best Practices Analyzer** from the Windows Start menu. <br/>1. On the **Welcome** page, click **Next**. <br/>2. On the **New best practices scan** page, click **Start scan**. <br/>3. If an **Office 365 Credentials** dialog appears, enter your Office 365 account and password. <br/>4. Wait for the scan to complete.|
Step 4: Learn more about any warnings or failures.|1. On the **Best practices scan results summary** page, click **View details** to open the **Detailed scan results** page, or click **Save scan results**, save the results to a HTML file, and open the file. <br/>2. Click the **Learn more** link next to any problems that are detected. <br/><br/> **Note**: If you saved your results, and you can't see the **Learn more** links in the HTML file, click **Allow blocked content** in the popup: **Internet Explorer restricted this webpage from running scripts or ActiveX controls**.|

## Next steps

After fixing any reported problems, you can run the Office 365 Best Practices Analyzer again from your Exchange 2013 server.
