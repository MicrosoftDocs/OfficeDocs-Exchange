---
title: 'Not in schema master site/domain_NotInSchemaMasterSite: Exchange 2013 Help'
TOCTitle: Not in schema master site/domain_NotInSchemaMasterSite
ms:assetid: 3aafd22a-d0f0-4120-a325-886fb2eb43ef
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.notinschemamastersite(v=EXCHG.150)
ms:contentKeyID: 46628868
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Not in schema master site/domain\_NotInSchemaMasterSite

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft® Exchange Server 2007 setup cannot continue because the computer that is running setup is not in the same Active Directory® directory service site or domain as the server that is assigned the domain schema master role, also known as flexible single master operations or FSMO.

Exchange 2007 setup requires the domain controller that serves as the domain schema master to be in the same site and domain as the local computer that is running Exchange setup.

The domain schema master controls all updates and modifications to the Active Directory schema.

To resolve this issue, run Exchange Server 2007 setup using the **/prepareschema** and **/prepareAD** switches from the same site and domain as the domain schema master.

For more information about the **/prepareschema** and **/prepareAD** setup switches, see the Exchange 2007 product documentation topic "How to Prepare Active Directory and Domains" (<https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb125224(v=exchg.80)>)

You can use the Schema Master tool to identify the role. However, the Schmmgmt.dll DLL must be registered in order to make the Schema tool available as an MMC snap-in.

**To view the current schema master**

1. At a command prompt, type **regsvr32 schmmgmt.dll**

    > [!NOTE]
    > <STRONG>RegSvr32</STRONG> has been successfully registered when the following dialog box is displayed:<BR>DllRegisterServer in schmmgmt.dll succeeded.

2. To open a new management console, click **Start**, click **Run**, and then type **mmc**.

3. On the Console menu, click **Add/Remove Snap-in**.

4. Click **Add** to open the **Add Standalone Snap-in** dialog box.

5. Select **Active Directory Schema**, and then click **Add**.

6. "Active Directory Schema" is displayed in the Add/Remove snap-in. Click **Close**, and then click **OK** to return to the console.

7. Select **Active Directory Schema** so that the **Classes** and **Attributes** sections are displayed on the right side.

8. Right-click **Active Directory Schema** and then click **Operations Master**.

9. The current schema master is displayed

After you identify the current schema master, determine which subnet the schema master is located in. Then, use one of the following methods to install Exchange:

  - Modify the subnet on the Exchange server to move it into the site in which the schema master is located. Then, install Exchange.

  - Temporarily force a site membership change on the Exchange server, and then install Exchange. After Exchange is installed, return the Exchange server to its original site.

**To force site membership**

1. On the server on which you want to install Exchange, start Registry Editor. To do this, click **Start**, click **Run**, type **regedit**, and then click **OK**.

2. Locate the following registry subkey:

    **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\Netlogon\\Parameters**

3. Create the following new **String** value:

    Value name: **SiteName**

    Value type: **REG\_SZ**

    Value data: **\<site\_that\_contains\_the\_schema\_master\>**

4. Exit Registry Editor, and then restart the Netlogon service. This action forces the Exchange server to participate in the site that you specified.

5. Install Exchange.

6. Remove the registry entry that you added in step 3.

7. Restart the Netlogon service. This action returns Exchange to the original site.
