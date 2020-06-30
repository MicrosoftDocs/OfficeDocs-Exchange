---
title: 'Prepare mailboxes for cross-forest moves using sample code: Exchange 2013 Help'
TOCTitle: Prepare mailboxes for cross-forest moves using sample code
ms:assetid: f35ac7a5-bb84-4653-b6d0-65906e93627b
ms:mtpsurl: https://technet.microsoft.com/library/Ee861124(v=EXCHG.150)
ms:contentKeyID: 49360517
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Prepare mailboxes for cross-forest moves using sample code

_**Applies to:** Exchange Server 2013_

Microsoft Exchange 2013 supports mailbox moves and migrations using the **New-MoveRequest** and **New-MigrationBatch** cmdlets. You can also move the mailbox via the Exchange admin center (EAC). You can move a mailbox from a source Exchange forest to a target Exchange 2010 forest.

To run **New-MoveRequest**, a mail user must exist in the target Exchange forest and the mail user must have a minimum set of required Active Directory attributes. You can create the required mail user in the target Exchange forest by customizing your Microsoft Identity Lifecycle Manager (ILM) 2007 deployment. The ILM-based rules extension sample code described in this topic demonstrates how to customize your current ILM deployment to create the required mail-enabled users in the target Exchange 2013 forest.

For more information about preparing for cross-forest moves, including descriptions of the required Active Directory attributes, see [Prepare mailboxes for cross-forest move requests](prepare-mailboxes-for-cross-forest-move-requests-exchange-2013-help.md).

## What do you need to know before you begin?

- Download the sample code from the [Prepare for Online Mailbox Move](https://www.microsoft.com/download/details.aspx?id=17741) page in the Microsoft Download Center.

- To run the sample code, you need ILM 2007 Feature Pack 1 Service Pack 1 (SP1).

- You also need the following:

  - A source forest running Exchange 2013, where the mailbox currently resides.

  - A target forest with Exchange 2013 installed, where the mailbox will be moved to.

- To connect to the Exchange 2013 target forest, you must have the appropriate permission to call the **UpdateRecipient** cmdlet. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Install the ILM sample code

1. In Microsoft Visual Studio 2008, open Microsoft.Exchange.Sample.OneWayGALSync.sln to view the sample code. The sample code includes the following:

   - Microsoft.MetadirectoryServicesEx.dll is the binary file that is shipped with ILM 2007 FP1 SP1 under "\\Program Files\\Microsoft Identity Integration Server\\Bin\\Assemblies". It's referenced by the sample code.

   - OneWaySync.xml is referenced by the sample code.

   - The ILMServerConfig folder contains the ILM configuration files for the source management agent (MA), target MA, and the ILM Metaverse (MV).

   - Microsoft.Exchange.Sample.OneWayGALSync.MARules.dll and Microsoft.Exchange.Sample.OneWayGALSync.MVRules.dll (built from the sample code) are under "\\obj\\Debug"

2. On the ILM server, copy the following to \\Program Files\\Microsoft Identity Integration Server\\Extensions:

   - OneWaySync.xml

   - Microsoft.Exchange.Sample.OneWayGALSync.MARules.dll

   - Microsoft.Exchange.Sample.OneWayGALSync.MVRules.dll

3. Edit the file OneWaySync.xml that you copied to the ILM Extensions folder in step 1 to specify the distinguishedName (DN) of the TargetOU container in the target Exchange forest in which you want to create the mail users. You can use LDP.exe or ADSIEdit.exe to browse for the TargetOU container if you don't know what its name is.

   > [!NOTE]
   > If you're using this sample together with ILM GalSync 2007 exclude this container from the list of containers managed by GalSync2007.

4. On the ILM Identity Manager Console, go to **File** \> **Import Server Configuration** to import the ILM server configuration from the folder ILMServerConfig. This action will import two Active Directory Management Agents along with the Metaverse schema and the provisioning rule.

   > [!NOTE]
   > During the import, you must provide the forest name and credentials and match the partitions of the imported Active Directory Management Agent (ADMA) to the partition name in your configuration for both the source and target ADMAs.

5. For the ADMA to support the Exchange 2013 target forest, on the **Create Management Agent** page, on the **Configure Extensions** pane, select **Exchange 2013** in the **Provision for** drop-down and then enter the remote Windows PowerShell URI of an Exchange 2010 Client Access server in **Exchange 2013 RPS URI**.

   **Create Management Agent page**

   ![Management Agent Exchange 2010 provisioning](images/Aa998597.8f403cda-e5e4-4edf-887f-c1ed46cee3f5(EXCHG.150).gif "Management Agent Exchange 2010  provisioning")

6. On the ILM Identity Manager Console on the **Create Management Agent** pane, open the **Properties** for the Source Forest Management Agent. Select the **Configure Directory Partitions** wizard, and then click **Containers** to select the container that will contain the mailboxes you will be moving to the target forest. Clear the selections for all other containers, that is, scope the management agent to only manage this one container. Similarly, for the target forest MA, select the container to which mail-enabled users will be provisioned, that is, the TargetOU specified in step 2.

   > [!NOTE]
   > If you're using this sample together with ILM GalSync 2007, exclude both of these containers from the list of containers managed by GalSync 2007.

7. Perform an initial Full Import (stage only) on the target MAs so that ILM can discover the TargetOU specified in step 2.

## Step 2: Create mail user in target Exchange forest

Now that you've installed the sample code, use the following procedure to create the required mail user in the target Exchange forest so that **New-MoveRequest** can be run to perform an online mailbox move.

1. In the source forest, use the Exchange admin center to create mailbox users in the container selected in step 4 of "Install the ILM sample code". You can also use Active Directory Users and Computers to move existing mailbox users to the container.

2. Perform Delta Import and Delta Sync run on the source MA to discover the mailboxes added to the source container, and provision mail users to the target MA.

3. Perform Export run on the target MA to export the mail users provisioned in step 1 to the target Active Directory.

4. Perform Delta Import on the target MA to confirm the changes exported in step 2.

5. In the target forest, open the Exchange Management Shell and use the **New-MoveRequest** cmdlet to move mailboxes from the source forest.

## How do you know this worked?

From the target forest, verify that the users that you moved from the source forest are present in the target forest.
