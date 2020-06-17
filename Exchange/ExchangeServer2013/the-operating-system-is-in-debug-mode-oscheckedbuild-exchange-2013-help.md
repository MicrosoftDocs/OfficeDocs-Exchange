---
title: 'The operating system is in debug mode_OSCheckedBuild: Exchange 2013 Help'
TOCTitle: The operating system is in debug mode_OSCheckedBuild
ms:assetid: 93a1380f-1388-494d-8f78-92dfefd069bd
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.oscheckedbuild(v=EXCHG.150)
ms:contentKeyID: 46629035
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The operating system is in debug mode\_OSCheckedBuild

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

The Microsoft® Exchange Server Analyzer Tool queries the **Win32\_OperatingSystem** Microsoft Windows® Management Instrumentation (WMI) class to determine whether a value is set for the **Debug** property. If the value for this key on an Exchange Server computer is set to **True**, an error is displayed.

Windows debug mode is toggled by adding the **/debug** parameter in the Boot.ini file. When **/debug** is specified in the Boot.ini file of a Windows Server computer, the kernel debugger is loaded during startup and kept in memory at all times. This enables a support professional to dial in to the system being debugged and break in to the debugger, even when the system is not suspended at a Kernel STOP screen. Unlike the **/crashdebug** switch, the **/debug** switch uses the COM port whether you are debugging or not. This switch is used when debugging problems that are regularly reproducible. It is likely that the **/debug** parameter was set as a means to troubleshoot a problem, and was unintentionally left set.

Because of the additional processes that are being run, performance suffers greatly. Therefore, running Exchange Server on a computer where Windows is running in debug mode is not recommended.

To eliminate this error, edit the Boot.ini file and remove the **/debug** parameter.

## To correct this error

1. In Windows Explorer, navigate to the System Partition. This is the partition that holds the hardware specific files such as Boot.ini and NTLDR.

2. If you cannot see the Boot.ini file, it could be because the **Folder Options** are set to **Hide protected operating system files**. If this is the case, in the Windows Explorer window, click **Tools**, click **Folder Options**, and then click **View**. Clear the **Hide protected operating system files (Recommended)** check box. When prompted, click **Yes**.

3. When the Boot.ini file is visible in Windows Explorer, right-click the file, click **Open With**, and then select **Notepad** to open the file.

4. In the **\[Operating Systems\]** section, remove the **/debug** parameter.

5. Save and close the file, and then restart the Exchange Server computer for the change to take effect.

For more information about the parameters that can be used in the Boot.ini file, see the Microsoft Knowledge Base article 833721, [Available switch options for the Windows XP and Windows Server 2003 Boot.ini files](https://support.microsoft.com/help/833721).
