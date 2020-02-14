---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because MachinePolicy or UserPolicy (or both) are defined in the ExecutionPolicy GPO.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.PowerShellExecutionPolicyCheckSet
ms.author: v-mapenn
ms.assetid: 63de83e2-9a6b-4f57-85b9-df445bea18dd
ms.reviewer:
title: ExecutionPolicy GPO is defined [PowerShellExecutionPolicyCheckSet]
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# ExecutionPolicy GPO is defined [PowerShellExecutionPolicyCheckSet]

Exchange Setup can't continue because it detected that the **ExecutionPolicy** Group Policy Object (GPO) defines one or both of the following policies:

- **MachinePolicy**

- **UserPolicy**

It doesn't matter how the policies have been defined; it only matters that they have been defined.

Exchange Setup stops and disables the Windows Management Instrumentation (WMI) service. When either of these policies are defined, the WMI service needs to be enabled to run a Windows PowerShell script. If the policies are defined and the WMI service is stopped, Setup will fail and the server will be left in an inconsistent state.

To allow Setup to continue, you need to temporarily remove any definition of **MachinePolicy** or **UserPolicy** in the **ExecutionPolicy** GPO:

```PowerShell
Set-ItemProperty -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell -Name ExecutionPolicy –Value ""
```

Having problems? Ask for help in the [Exchange forums](https://social.technet.microsoft.com/Forums/office/home?category=exchangeserver).
