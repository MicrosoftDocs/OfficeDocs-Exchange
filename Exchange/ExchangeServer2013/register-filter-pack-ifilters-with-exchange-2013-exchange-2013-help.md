---
title: 'Register Filter Pack IFilters with Exchange 2013: Exchange 2013 Help'
TOCTitle: Register Filter Pack IFilters with Exchange 2013
ms:assetid: 0338980f-3a64-49d3-bc3c-bf6f10f88cb4
ms:mtpsurl: https://technet.microsoft.com/library/JJ837174(v=EXCHG.150)
ms:contentKeyID: 49940598
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Register Filter Pack IFilters with Exchange 2013

_**Applies to:** Exchange Server 2013_

Transport rules with attachment scanning conditions perform text extraction when analyzing the content of attachments. Exchange 2013 can scan most commonly used attachment types natively. Additional attachment types can be included by registering IFilters with Exchange 2013. This topic shows you how to register IFilters released by Microsoft and third-party providers.

After you register an IFilter for a specific file type, transport rules with attachment processing conditions will be able to scan these attachments. As a result, these file types will no longer trigger the *AttachmentIsUnsupported* condition.

> [!WARNING]
> The procedures listed in this topic involve modifying the registry on your Exchange servers. Incorrectly editing the registry can cause serious problems that may require you to reinstall your operating system. Problems resulting from editing the registry incorrectly may not be able to be resolved. Before editing the registry, back up any valuable data.<BR>These procedures also require you to stop and restart the Microsoft Exchange Transport service on your Mailbox servers.

For additional management tasks related to Transport rules, see [Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes per server.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange server configuration settings" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- You must perform the procedures below on servers that already have Exchange 2013 Mailbox server role installed. If you add additional Mailbox servers after you perform these procedures, you must perform them again on the newly provisioned servers.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Register the Microsoft Office 2010 Filter Pack

By default, the following Office file types aren't supported by Exchange transport rules:

- Office OneNote

- Office Publisher

If you want to support these files, you must deploy the Microsoft Office 2010 Filter Pack. This Filter Pack isn't deployed during Exchange 2013 Setup and isn't a prerequisite for deployment.

## Deploy the Microsoft Office 2010 Filter Pack

Deploying the Office 2010 Filter Pack consists of two main steps:

- Downloading and installing the Filter Pack, which registers the IFilters with Windows (Search).

- Modifying the registry so the IFilters are also registered with Exchange 2013. This allows Exchange to support attachment scanning for the file formats.

> [!IMPORTANT]
> You must perform this procedure on all Mailbox servers in your organization.

1. Download and save the Microsoft Office 2010 Filter Pack (`FilterPack64bit.exe`) from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=17062).

2. Run the `FilterPack64bit.exe` file on your Mailbox server and follow the instructions to complete the installation.

3. Start Registry Editor and locate the following registry subkey:

    ```powershell
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v15\HubTransportRole\CLSID
    ```

4. Under **CLSID**, add a subkey for OneNote files as follows:

    1. Right-click **CLSID**, point to **New**, and then click **Key**.

    2. Change the name of the new key to `{B8D12492-CE0F-40AD-83EA-099A03D493F1}`.

    3. Click the key you just created and set the **(Default)** value to where you installed the Office 2010 Filter Pack. By default, the filter pack gets installed at `C:\Program Files\Common Files\Microsoft Shared\Filters\ONIFilter.dll`.

    4. Right click **{B8D12492-CE0F-40AD-83EA-099A03D493F1}**, point to **New**, and then click **String Value**.

    5. Name the new string value `ThreadingModel` and set it to `Both`.

5. Under **CLSID**, add a subkey for Publisher files as follows:

    1. Right-click **CLSID**, point to **New**, and then click **Key**.

    2. Change the name of the new key to `{A7FD8AC9-7ABF-46FC-B70B-6A5E5EC9859A}`.

    3. Click the key you just created and set the **(Default)** value to where you installed the Office 2010 Filter Pack. By default, the filter pack gets installed at `C:\Program Files\Common Files\Microsoft Shared\Filters\PUBFILT.dll`.

    4. Right-click **{A7FD8AC9-7ABF-46FC-B70B-6A5E5EC9859A}**, point to **New**, and then click **String Value**.

    5. Name the new string value `ThreadingModel` and set it to `Both`.

6. Locate the following registry key:

    ```powershell
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v15\HubTransportRole\filters
    ```

7. Under **filters**, add a subkey for .one extensions as follows.

    1. Right-click **filters**, point to **New**, and then click **Key**.

    2. Change the name of the new key to `.one`.

    3. Click the key you just created and set the **(Default)** value to `{B8D12492-CE0F-40AD-83EA-099A03D493F1}`.

8. Under **filters**, add a subkey for .pub extensions as follows:

    1. Right-click **filters**, point to **New** and then click **Key**.

    2. Change the name of the new key to `.pub`.

    3. Click the key you just created and set the **(Default)** value to `{A7FD8AC9-7ABF-46FC-B70B-6A5E5EC9859A}`.

9. Close Registry Editor.

10. On your Mailbox server, stop and then restart the following services in the specified order:

    1. Stop the Microsoft Exchange Transport service.

    2. Stop the Microsoft Filtering Management Service.

    3. Start the Microsoft Filtering Management Service.

    4. Start the Microsoft Exchange Transport service.

## How do you know this worked?

To verify that you have successfully registered the Microsoft Office 2010 Filter Pack IFilters, do the following:

1. Create a Transport rule with the following properties. For detailed instructions about how to create Transport rules, see [Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md).

   - The sender is your mailbox.

   - Any attachment's content includes "Testing IFilters".

   - Generate an incident report and send it to your mailbox.

2. Create a OneNote file that contains the phrase "Testing IFilters", attach it to a new email message, and send it to yourself.

3. Verify that you receive a Transport rule incident report for the rule you just created. This confirms that the rules engine was able to analyze the contents of the OneNote file.

4. Repeat Steps 2 and 3 with a Publisher file.

## Register third-party IFilters to support additional file formats

You can extend the attachment scanning capability for additional file types by registering additional third-party IFilters. Support for additional files can be added by installing and registering the file type's IFilter on each of your Mailbox servers.

> [!IMPORTANT]
> Microsoft hasn't tested third-party IFilters with transport rules, therefore we recommend that you deploy and test any third-party IFilters in a test environment before deploying into your production environment.

## Deploy the Adobe PDF IFilter

This procedure shows how to deploy the [Adobe PDF IFilter](https://www.adobe.com/support/downloads/detail.jsp?ftpid=4025) to support processing of PDF attachments in transport rules.

> [!NOTE]
> By default, Exchange 2013 supports the scanning of PDF files in transport rules. The PDF example here is used simply to illustrate how you can extend support for additional file types using third-party IFilters.

1. Download the [Adobe PDF IFilter](https://www.adobe.com/support/downloads/detail.jsp?ftpid=4025)and then follow the installation instructions.

2. Start Registry Editor and locate the following subkey:

   ```console
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v15\HubTransportRole\CLSID
   ```

3. Under **CLSID**, add a subkey for PDF files as follows:

   1. Right-click **CLSID**, point to **New**, and then click **Key**.

   2. Change the name of the new key to `{E8978DA6-047F-4E3D-9C78-CDBE46041603}`.

      > [!NOTE]
      > Each IFilter has a unique class ID (CLSID). You can find the CLSID in the installation documentation for the IFilter you're registering or by searching for the file extension under the <CODE>HKEY_CLASSES_ROOT\CLSID</CODE> key in the registry.

   3. Click the key you just created and set the **(Default)** value to where you installed the PDF IFilter. By default, the PDF IFilter is installed at `C:\Program Files\Adobe\Adobe PDF IFilter 9 for 64-bit platforms\bin\PDFFilter.dll`.

4. Locate the following registry key:

    ```console
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ExchangeServer\v15\HubTransportRole\filters
    ```

5. Under **filters**, add a subkey for .pdf extensions as follows:

   1. Right-click **filters**, point to **New**, and then click **Key**.

   2. Change the name of the new key to `.pdf`.

   3. Click the key you just created and set the **(Default)** value to `{E8978DA6-047F-4E3D-9C78-CDBE46041603}`.

6. Close Registry Editor.

7. On your Mailbox server, stop and restart the following services in the specified order:

   1. Stop the Microsoft Exchange Transport service.

   2. Stop the Microsoft Filtering Management Service.

   3. Start the Microsoft Filtering Management Service.

   4. Start the Microsoft Exchange Transport service.

## How do you know this worked?

Use the same procedure listed in the How do you know this worked? section earlier in this topic, substituting Publisher files with Adobe PDF files.

## For more information

[Use transport rules to inspect message attachments](use-transport-rules-to-inspect-message-attachments-exchange-2013-help.md)

[Mail flow or transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)

[Transport rule conditions (predicates)](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md)

[Transport rule actions](mail-flow-rule-actions-in-exchange-2013-exchange-2013-help.md)
