---
title: 'Supported character sets for remote domains: Exchange 2013 Help'
TOCTitle: Supported character sets for remote domains
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 66023a62-1fd3-4019-be2b-4e7147db148a
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Supported character sets for remote domains in Exchange 2013

_**Applies to:** Exchange Server 2013_

The following character sets can be specified for messages sent to remote domains.

- In the Exchange admin center (EAC), on the **Remote domain** settings page, select the name from the **MIME character set** and **Non-MIME character set** drop-down lists.

- In the Shell, use the value in the Name column in the following table for the _CharacterSet_ parameter or _NonMimeCharacterSet_ parameter in the [Set-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/set-remotedomain) cmdlet.

**Supported character sets for remote domain configuration**

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
