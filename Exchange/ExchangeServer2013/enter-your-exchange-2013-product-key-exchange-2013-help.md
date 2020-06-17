---
title: 'Enter your Exchange 2013 product key: Exchange 2013 Help'
TOCTitle: Enter your Exchange 2013 product key
ms:assetid: ccb14685-4bdc-42a4-a985-35cd2a1a415c
ms:mtpsurl: https://technet.microsoft.com/library/Bb124582(v=EXCHG.150)
ms:contentKeyID: 50643913
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Servers.EnterProductKeyWizardForm.EnterProductKeyWizardPage
---

# Enter your Exchange 2013 product key

_**Applies to:** Exchange Server 2013_

A product key tells Exchange Server 2013 that you've purchased a Standard or Enterprise Edition license. If the product key you purchased is for an Enterprise Edition license, it lets you mount more than five databases per server in addition to everything that's available with a Standard Edition license. If you want to read more about Exchange licensing, see [Exchange 2013: editions and versions](exchange-2013-editions-and-versions-exchange-2013-help.md).

If you don't enter a product key, your server is automatically licensed as a trial edition. The trial edition functions just like an Exchange Standard Edition server and is helpful if you want to try out Exchange before you buy it, or to run tests in a lab. The only difference is that you can only use an Exchange server licensed as a trial edition for up to 180 days. If you want to keep using the server beyond 180 days, you'll need to enter a product key or the Exchange admin center (EAC) will start to show reminders that you need to enter a product key to license the server.

> [!TIP]
> We've noticed some visitors to this page are looking for information on how to install or activate Office. If that's you, check out these pages:
> <UL>
> <LI>
> <P><A href="https://go.microsoft.com/fwlink/p/?linkid=403360">Install Office</A></P>
> <LI>
> <P><A href="https://go.microsoft.com/fwlink/p/?linkid=403361">Need help with your Office product key?</A></P></LI></UL>If you want to enter a product key on an Exchange 2010 server, go to <A href="https://go.microsoft.com/fwlink/p/?linkid=403370">Enter an Exchange 2010 product key</A>.<BR>If you want to enter a product key on an Exchange 2013 server, you're in the right place! Read on.

## What do you need to know before you begin?

- Estimated time to complete this procedure: less than 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Product key" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- If you're licensing an Exchange server that's running the Mailbox server role, you'll need to restart the Microsoft Exchange Information Store service on the server after you enter the product key.

- If you want to upgrade an Exchange server from a Standard Edition license to an Enterprise Edition license, follow the steps in this topic.

- If you want to downgrade an Exchange server from an Enterprise Edition license to a Standard Edition license, you need to reinstall Exchange.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to enter the product key

1. Open the EAC by browsing to https://\<*Client Access server name*\>/ecp.

2. Enter your username and password in **Domain\\user name** and **Password**, and then click **Sign in**.

3. Go to **Servers** \> **Servers**. Select the server you want to license, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. (Optional) If you want to upgrade the server from a Standard Edition license to an Enterprise Edition license, on the **General** page, select **Change product key**. You'll only see this option if the server is already licensed.

5. On the **General** page, enter your product key in the **Enter a valid product key** text boxes.

6. Click **Save**.

7. If you licensed an Exchange server running the Mailbox server role, do the following to restart the Microsoft Exchange Information Store service:

    1. Open **Control Panel**, go to **Administrative Tools**, and then open **Services**.

    2. Right-click on **Microsoft Exchange Information Store** and click **Restart**.

## Use the Shell to enter the product key

This example uses the **set-ExchangeServer** cmdlet to enter the product key.

> [!NOTE]
> You can run this command again on the same server to upgrade it from a Standard Edition license to an Enterprise Edition license.

```powershell
Set-ExchangeServer ExServer01 -ProductKey aaaaa-aaaaa-aaaaa-aaaaa-aaaaa
```

For detailed syntax and parameter information, see [Set-ExchangeServer](https://docs.microsoft.com/powershell/module/exchange/Set-ExchangeServer).

If you licensed an Exchange server running the Mailbox server role, do the following to restart the Microsoft Exchange Information Store service:

1. Open **Control Panel**, go to **Administrative Tools**, and then open **Services**.

2. Right-click **Microsoft Exchange Information Store** and click **Restart**.

## How do you know this worked?

To use the EAC to verify that you've successfully licensed the server as Standard Edition or Enterprise Edition, do the following:

1. Enter your username and password in **Domain\\user name** and **Password**, and then click **Sign in**.

2. Go to **Servers** \> **Servers**.

3. Select the server you want to view, and then look in the server details pane. If the product key has been accepted, **Licensed** will appear along with the Exchange 2013 edition.

To use the Shell to verify that you've successfully licensed the server as Standard Edition or Enterprise Edition, do the following:

1. Open the Shell.

2. Run the following command to view the licensing status of a specific Exchange server.

    ```powershell
    Get-ExchangeServer ExServer01 | Format-Table Edition,*Trial*
    ```

3. (Optional) Run the following command to view the licensing status of all Exchange servers in your organization.

    ```powershell
    Get-ExchangeServer | Format-Table Name, Edition, *Trial* -Auto
    ```
