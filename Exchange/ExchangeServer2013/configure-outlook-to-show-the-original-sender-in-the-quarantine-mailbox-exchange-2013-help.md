---
title: 'Configure Outlook to show the original sender in the quarantine mailbox'
TOCTitle: Configure Outlook to show the original sender in the quarantine mailbox
ms:assetid: 9249425d-1b06-48a0-ad95-c4eefb641ff4
ms:mtpsurl: https://technet.microsoft.com/library/Ee861109(v=EXCHG.150)
ms:contentKeyID: 49345054
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Outlook to show the original sender in the quarantine mailbox

_**Applies to:** Exchange Server 2013_

Spam quarantine is a feature of the Content Filter agent that reduces the risk of losing legitimate messages. Spam quarantine provides a temporary storage location for messages that are identified as spam and that shouldn't be delivered to a user mailbox inside the organization.

When a message meets the spam quarantine threshold, it's wrapped in a non-delivery report (NDR) and delivered to the spam quarantine mailbox. Because the quarantined messages are stored as NDRs in the quarantine mailbox, the postmaster address of your organization will be listed as the From: address for all messages. However, having the original sender address, the original recipient address, and the original spam confidence level (SCL) in the field list would make it easier to locate the message you want to recover.

By default, you can't select these fields in Microsoft Outlook. Before you can add these fields in the message view, you must first create an Outlook form that adds the original sender, original recipient, and original SCL as optional fields you can select. After you create this custom form, you can configure Outlook to display these fields in the message view.

## What do you need to know before you begin?

- Estimated time to complete this procedure: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox access" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- This procedure requires that you've configured the anti-spam quarantine mailbox. For more information, see [Configure a spam quarantine mailbox](configure-a-spam-quarantine-mailbox-exchange-2013-help.md).

- To access the quarantine mailbox, you need to configure an Outlook profile for that mailbox and then open the mailbox using Outlook. For more information about configuring and using multiple Outlook profiles, see [Overview of Outlook e-mail profiles](https://support.office.com/article/f544c1ba-3352-4b3b-be0b-8d42a540459d).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Use Notepad to create a custom Outlook form

1. Open Notepad, and copy the following code into the document.

   ```powershell
   [Description]
   MessageClass=IPM.Note
   CLSID={00020D31-0000-0000-C000-000000000046}
   DisplayName=Quarantine Extension Form
   Category=Standard
   Subcategory=Form
   Comment=This form allows the Original Sender (ReceivedRepresentingEmailAddress), Original Recipient (To), and Original SCL (OriginalScl) values to be viewed as columns.
   LargeIcon=IPML.ico
   SmallIcon=IPMS.ico
   Version=3.0
   Locale=enu
   Hidden=1
   Owner=Microsoft Corporation
   Contact=Your Name

   [Platforms]
   Platform1=Win16
   Platform2=NTx86
   Platform9=Win95

   [Platform.Win16]
   CPU=ix86
   OSVersion=Win3.1

   [Platform.NTx86]
   CPU=ix86
   OSVersion=WinNT3.5

   [Platform.Win95]
   CPU=ix86
   OSVersion=Win95

   [Properties]
   Property01=ReceivedRepresentingEmailAddress
   Property02=DisplayTo
   Property03=OriginalScl

   [Property.ReceivedRepresentingEmailAddress]
   Type=31
   NmidInteger=0x0078
   DisplayName=ReceivedRepresentingEmailAddress

   [Property.DisplayTo]
   Type=31
   NmidInteger=0x0E04
   DisplayName=DisplayTo

   [Property.OriginalScl]
   Type=3
   NmidPropset={41F28F13-83F4-4114-A584-EEDB5A6B0BFF}
   NmidString=OriginalScl
   DisplayName=OriginalScl

   [Verbs]
   Verb1=1

   [Verb.1]
   DisplayName=&Open
   Code=0
   Flags=0
   Attribs=2

   [Extensions]
   Extensions1=1

   [Extension.1]
   Type=31
   NmidPropset={00020D0C-0000-0000-C000-000000000046}
   NmidInteger=1
   Value=1000000000000000
   ```

2. Save the file in your Office Forms folder using the following values:

   - **Path**: *\<Office Install Path\>*\\\<*OfficeVersion\>*\\Forms\\*\<LCID\>*

     - *\<Office Install Path\>*: For 32-bit versions of Office on 32-bit versions of Microsoft Windows, or 64-bit versions of Office on 64-bit versions of Windows, the default path is `C:\Program Files\Microsoft Office`. For 32-bit versions of Office on 64-bit versions of Windows, the default path is `C:\Program Files (x86)\Microsoft Office`.

     - *\<OfficeVersion\>*: For Outlook 2007, the value is `Office12`. For Outlook 2010, the value is `Office14`. For Outlook 2013, the value is `Office15`.

     - *\<LCID\>*: This is your locale ID (LCID) value. For example, the LCID for US English is 1033. For more information, see [Language identifiers and OptionState Id values in Office](https://docs.microsoft.com/deployoffice/office2016/language-identifiers-and-optionstate-id-values-in-office-2016).

   - **Name**: For the rest of this procedure, assume the file is named `QTNE.cfg`. The name of the file isn't important, but be sure to enclose the value in quotation marks so the file is saved as QTNE.cfg and not QTNE.cfg.txt.

   For example, for a 32-bit US English version of Outlook 2013 installed on a 64-bit version of Windows, save the file as:

   ```powershell
   "C:\Program Files (x86)\Microsoft Office\Office15\Forms\1033\QTNE.cfg"
   ```

> [!NOTE]
> If Windows User Access Control (UAC) prevents you from saving the file in the correct location, save it first to a temporary location, and then copy it.

## Step 2: Configure Outlook to use the custom Outlook form

Use one of the following procedures based on the version of Outlook that's installed on your computer.

## Configure Outlook 2010 or Outlook 2013

1. In Outlook 2010 or Outlook 2013, click **File** \> **Options** \> **Advanced**.

2. In the **Developers** section, click **Custom Forms**.

3. In the **Options** dialog box, click **Manage Forms**.

4. In the **Forms Manager** dialog box, click **Install**. Browse to the location of the `QTNE.cfg` file, select it, and click **Open**. In the **Form Properties** dialog box, review the information, and then click **OK** to install the Quarantine Extension Form in your Personal Forms library.

5. In the **Forms Manager** dialog box, click **Close**. Click **OK** twice to close the remaining dialog boxes and return to the main Outlook interface.

6. On the **Home** tab in the **Mail** view of the Inbox, right-click the column heading row (you may need to expand the width of the message list to see the columns), and then select **View Settings**.

7. In the **Advanced View Settings** dialog box, click **Columns**.

8. In the **Show Columns** dialog box, in the **Select available columns from** drop-down list, scroll to the end of the list and select **Forms**.

9. In the **Select Enterprise forms for this folder** dialog box, in the **Selected Forms** field, select **Message** and click **Remove**. In the **Personal Forms** field, select **Quarantine Extension Form**, and then click **Add**. When you are finished, click **Close**.

10. In the **Show Columns** dialog box, in the **Available Columns** section, select one or more of the following fields and click **Add** after each field you select.

    - **ReceivedRepresentingEmailAddress**: Original sender

    - **DisplayTo**: Original recipient (note that this appears as **To** after you add it)

    - **OriginalScl**: Original SCL

    Use the **Move Up** or **Move Down** buttons to position the columns in the view. For best results, position the three new fields after the **Attachment** field, and before the **From** field. When you are finished, click **OK** twice to return to the main Outlook interface.

## Configure Outlook 2007

1. In Outlook 2007, click **Tools** \> **Options**.

2. In the **Options** dialog box, click the **Other** tab, and then under **General**, click **Advanced Options**.

3. In the **Advanced Options** dialog box, click **Custom Forms**, and then in the **Custom Forms** dialog box, click **Manage Forms**.

4. In the **Forms Manager** dialog box, click **Install**. Browse to the location of the `QTNE.cfg` file, select it, and click **Open**. In the **Form Properties** dialog box, review the information, and then click **OK** to install the Quarantine Extension Form in your Personal Forms library.

5. Close the **Forms Manager** dialog box, and then click **OK** three times to close the remaining dialog boxes and return to the main Outlook 2007 interface.

6. In the **Mail** view of the Inbox, right-click the column heading row (you may need to expand the width of the message list to see the columns), and then select **Customize Current View**.

7. In the **Customize View: Messages** dialog box, click **Show Fields**.

8. In the **Show Fields** dialog box, in the **Select available fields from** drop-down list, scroll to the end of the list and select **Forms**.

9. In the **Select Enterprise forms for this folder** dialog box, in the **Selected Forms** field, select **Message** and click **Remove**. In the **Personal Forms** field, select **Quarantine Extension Form**, and then click **Add**. When you are finished, click **Close**.

10. In the **Show Fields** dialog box, in the **Available Fields** section, select one or more of the following fields and click **Add** after each field you select.

    - **ReceivedRepresentingEmailAddress**: Original sender

    - **DisplayTo**: Original recipient (note that this appears as **To** after you add it)

    - **OriginalScl**: Original SCL

    Use the **Move Up** or **Move Down** buttons to position the columns in the view. For best results, position the three new fields after the **Attachment** field, and before the **From** field. When you are finished, click **OK** twice to return to the main Outlook 2007 interface.

## How do you know this worked?

You know this procedure worked if you can see the original sender, original recipient, or original SCL values for quarantined messages in the spam quarantine mailbox using Outlook.
