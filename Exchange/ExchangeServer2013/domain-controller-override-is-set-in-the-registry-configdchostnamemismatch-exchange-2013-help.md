---
title: 'Domain Controller Override is set in the Registry'
TOCTitle: Domain Controller Override is set in the Registry_ConfigDCHostNameMismatch
ms:assetid: 3aef5470-d510-4b59-a4b6-36d274a984ae
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.configdchostnamemismatch(v=EXCHG.150)
ms:contentKeyID: 46628888
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Domain Controller Override is set in the Registry\_ConfigDCHostNameMismatch

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because its attempt to use the specified domain controller failed. A domain controller has been statically mapped in the registry.

Exchange 2007 setup requires that the domain controller specified in the setup command match the domain controller that has been statically mapped using a registry override.

To resolve this issue, run setup again, specifying the statically mapped domain controller for the **/DomainController: \<***FQDN of the* *statically mapped domain controller***\>** parameter.
