---
title: 'S/MIME settings in Outlook on the web'
description: 'Summary: Learn about the considerations for using S/MIME in Outlook on the web in Exchange Server 2016 or Exchange Server 2019.'
ms.assetid: c7dee22c-9b5b-425c-91a9-d093204ff84e
monikerRange: exchserver-2016 || exchserver-2019
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
---

# S/MIME settings for Outlook on the web in Exchange Server

As an Exchange administrator, you can set up Outlook on the web (formerly known as Outlook Web App) to allow sending and receiving S/MIME-protected messages. Use the **Get-SmimeConfig** and **Set-SmimeConfig** cmdlets to view and manage this feature in the Exchange Management Shell. To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/exchange-server/open-the-exchange-management-shell).

For detailed syntax and parameter information, see [Get-SmimeConfig](https://docs.microsoft.com/powershell/module/exchange/encryption-and-certificates/get-smimeconfig) and [Set-SmimeConfig](https://docs.microsoft.com/powershell/module/exchange/encryption-and-certificates/set-smimeconfig).

::: moniker range="exchserver-2016"
S/MIME in Outlook on the web in Exchange 2016 is only supported in Internet Explorer.
::: moniker-end

::: moniker range="exchserver-2019"
S/MIME in Outlook on the web in Exchange 2019 is supported in Internet Explorer, Edge (non-Chromium version), and Google Chrome.

## Considerations for Chrome

To use S/MIME in Outlook on the web in the Google Chrome web browser, you (or another administrator) must set and configure the Chromium policy named **ExtensionInstallForcelist** to install the Microsoft S/MIME extension in Chrome. The policy value is `maafgiompdekodanheihhgilkjchcakm;https://outlook.office.com/owa/SmimeCrxUpdate.ashx`. And note that applying this policy requires domain-joined computers, so using S/MIME in Chrome effectively requires domain-joined computers.

For details about the **ExtensionInstallForcelist** policy, see [ExtensionInstallForcelist](https://cloud.google.com/docs/chrome-enterprise/policies/?policy=ExtensionInstallForcelist).

This step is a prerequisite for using Chrome; it does not replace the S/MIME control that's installed by users. Users are prompted to download and install the S/MIME control in Outlook on the web during their first use of S/MIME. Or, users can proactively go to **S/MIME** in their Outlook on the web settings to get the download link for the control.
::: moniker-end
