---
localization_priority: Normal
description: MIME and non-MIME character sets that admins can configure in remote domains (message formatting settings for external domains) in Exchange Online
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 66023a62-1fd3-4019-be2b-4e7147db148a
ms.reviewer: 
title: Supported character sets for remote domains in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Supported character sets for remote domains in Exchange Online

Remote domains define settings based on the destination domain of each email message. All organizations have a default remote domain named "Default" that's applied to the domain "*". The default remote domain applies the same settings to all email messages regardless of the destination domain. However, you can configure specific settings for a specific destination domain.

For more information about remote domains, see [Remote domains in Exchange Online](remote-domains.md).

For remote domain procedures, see [Manage remote domains in Exchange Online](manage-remote-domains.md).

The following table describes the character sets that you can configure in remote domains.

- In the Exchange admin center (EAC), go to **Mail flow** > **Remote domains**. Click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.png) to create a new remote domain or select the existing remote domain and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the settings window that opens, use the **MIME character set** and **Non-MIME character set** drop-down lists to select the character set.

- In Exchange Online PowerShell, use the value in the Name column in the following table for the _CharacterSet_ parameter or _NonMimeCharacterSet_ parameter on the [Set-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/set-remotedomain) cmdlet.

|**Name**|**Description**|
|:-----|:-----|
|big5|Chinese Traditional (Big5)|
|DIN_66003|German (IA5)|
|euc-jp|Japanese (EUC)|
|euc-kr|Korean (EUC)|
|GB18030|Chinese Simplified (GB18030)|
|gb2312|Chinese Simplified (GB2312)|
|hz-gb-2312|Chinese Simplified (HZ)|
|iso-2022-jp|Japanese (JIS)|
|iso-2022-kr|Korean (ISO)|
|iso-8859-1|Western European (ISO)|
|iso-8859-2|Central European (ISO)|
|iso-8859-3|Latin 3 (ISO)|
|iso-8859-4|Baltic (ISO)|
|iso-8859-5|Cyrillic (ISO)|
|iso-8859-6|Arabic (ISO)|
|iso-8859-7|Greek (ISO)|
|iso-8859-8|Hebrew (ISO)|
|iso-8859-9|Turkish (ISO)|
|iso-8859-13|Estonian (ISO)|
|iso-8859-15|Latin 9 (ISO)|
|koi8-r|Cyrillic (KOI8-R)|
|koi8-u|Cyrillic (KOI8-U)|
|ks_c_5601-1987|Korean (Windows)|
|NS_4551-1|Norwegian (IA5)|
|SEN_850200_B|Swedish (IA5)|
|shift_jis|Japanese (Shift-JIS)|
|utf-8|Unicode (UTF-8)|
|windows-1250|Central European (Windows)|
|windows-1251|Cyrillic (Windows)|
|windows-1252|Western European (Windows)|
|windows-1253|Greek (Windows)|
|windows-1254|Turkish (Windows)|
|windows-1255|Hebrew (Windows)|
|windows-1256|Arabic (Windows)|
|windows-1257|Baltic (Windows)|
|windows-1258|Vietnamese (Windows)|
|windows-874|Thai (Windows)|
