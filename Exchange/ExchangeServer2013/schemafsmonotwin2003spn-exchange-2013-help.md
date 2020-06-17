---
title: 'Schema master is not running Windows Server 2003 Service Pack 1 or later'
TOCTitle: The schema master is not running Windows Server 2003 Service Pack 1 or later_SchemaFSMONotWin2003SPn
ms:assetid: 644a85ca-7b36-4ed0-bd21-c64f2742df70
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.schemafsmonotwin2003spn(v=EXCHG.150)
ms:contentKeyID: 46628975
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The schema master is not running Windows Server 2003 Service Pack 1 or later\_SchemaFSMONotWin2003SPn

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because the domain controller assigned the Active Directory directory service schema master role, also known as flexible single master operations (FSMO), is not running Microsoft Windows Server 2003 Service Pack 1 (SP1) or a later version.

Exchange 2007 setup requires that the domain controller that serves as the schema FSMO run Windows Server 2003 SP1 or a later version.

The FSMO controls all updates and modifications to the Active Directory schema.

To resolve this issue, do one or more of the following:

  - Upgrade the FSMO domain controller to Windows Server 2003 SP1 or a later version and rerun Microsoft Exchange setup.

  - If there is a FSMO domain controller running Microsoft Windows Server 2003 Service Pack 1 (SP1) or a later version in the Exchange organization, run Exchange 2007 setup with the /domaincontroller parameter pointing to that FSMO domain controller:

    \[*/DomainController*, or */dc* *\<FQDN of domain controller\>*\]

    Use the */DomainController* parameter to specify the domain controller to use to read from and write to Active Directory during setup. You can use NetBIOS or the fully qualified domain name (FQDN) format.

To obtain the latest service pack for Windows Server 2003, see [KB889100](https://support.microsoft.com/help/889100).

For more information about Exchange Server 2007 Setup parameters, see "How to Install Exchange 2007 in Unattended Mode" ([https://go.microsoft.com/fwlink/?LinkId=86476](https://go.microsoft.com/fwlink/?linkid=86476)) in the Exchange Server 2007 product documentation.
