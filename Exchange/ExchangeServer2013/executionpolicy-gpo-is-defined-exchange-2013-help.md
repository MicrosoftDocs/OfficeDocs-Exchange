---
title: 'ExecutionPolicy GPO is defined: Exchange 2013 Help'
TOCTitle: ExecutionPolicy GPO is defined
ms:assetid: 63de83e2-9a6b-4f57-85b9-df445bea18dd
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.powershellexecutionpolicycheckset(v=EXCHG.150)
ms:contentKeyID: 61200286
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# ExecutionPolicy GPO is defined

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because it detected that the **ExecutionPolicy** Group Policy Object (GPO) defines one or both of the following policies:

  - **MachinePolicy**

  - **UserPolicy**

It doesn't matter how the policies have been defined. It only matters that they have been defined.

When you run Exchange 2013 Setup, Exchange stops and disables the Windows Management Instrumentation (WMI) service. When either of these policies are defined, the WMI service needs to be enabled to run a Windows PowerShell script. If the policies are defined and the WMI service is stopped, Setup will fail and the server will be left in an inconsistent state.

To allow Setup to continue, you need to temporarily remove any definition of **MachinePolicy** or **UserPolicy** in the **ExecutionPolicy** GPO:

```PowerShell
Set-ItemProperty -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell -Name ExecutionPolicy –Value ""
```

Having problems? Ask for help in the [Exchange forums](https://social.technet.microsoft.com/Forums/office/home?category=exchangeserver).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
