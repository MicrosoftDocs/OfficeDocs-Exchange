---
title: 'Modify, disable, or remove a sharing policy: Exchange 2013 Help'
TOCTitle: Modify, disable, or remove a sharing policy
ms:assetid: 714af42d-ca29-4bb4-ac48-f0b3d4fd1c15
ms:mtpsurl: https://technet.microsoft.com/library/JJ657460(v=EXCHG.150)
ms:contentKeyID: 49289300
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Modify, disable, or remove a sharing policy

_**Applies to:** Exchange Server 2013_

Sharing policies allow individual users in your Exchange organization to share calendar free/busy information with other federated Exchange organizations, non-federated Exchange organizations, and individual Internet users. During the course of normal operations, you may want to change some sharing policy properties, such as modifying sharing rules, changing the free/busy access level, temporarily disabling a sharing policy, or removing a sharing policy entirely.

To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md)

For details about how to create a sharing policy, see [Create a sharing policy](create-a-sharing-policy-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Calendar and Sharing Permissions" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to modify a sharing policy

1. Navigate to **organization** \> **sharing**.

2. Under **Individual Sharing**, select a sharing a policy, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **sharing policy**, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. In **sharing rule**, modify the sharing rules accordingly. You can change settings such as the domain you want to share information with and the sharing level for calendar appointments. When finished, click **rave** to close the **sharing rules** dialog box.

5. In **sharing policy**, click **save** to update the sharing policy.

## Use the EAC to set a sharing policy as the default sharing policy

1. Navigate to **organization** \> **sharing**.

2. Under **Individual Sharing**, select a sharing a policy, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **sharing policy**, select the **Make this policy my default sharing policy** check box.

4. Click **save** to update the sharing policy.

## Use the EAC to disable a sharing policy

1. Navigate to **Organization** \> **Sharing**.

2. Under **Individual Sharing**, select a sharing a policy.

3. In the **On** column, clear the check box for the sharing policy you want to disable.

## Use the EAC to remove a sharing policy

> [!IMPORTANT]
> Before you remove a sharing policy, the sharing policy must be removed from all user mailboxes.

1. Navigate to **organization** \> **sharing**.

2. Under **Individual Sharing**, select a sharing a policy, and then click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

3. In the warning, click **yes** to delete the sharing policy.

## Use the Shell to modify, disable or remove a sharing policy

- This example modifies the sharing policy Contoso for contoso.com, which is a domain outside your organization. This policy allows users in the Contoso domain to see simple free/busy information.

    ```powershell
    Set-SharingPolicy -Identity Contoso -Domains 'sales.contoso.com: CalendarSharingFreeBusySimple'
    ```

- This example adds a second domain to the sharing policy Contoso. When you're adding a domain to an existing policy, you must include any previously included domains.

    ```powershell
    Set-SharingPolicy -Identity Contoso -Domains 'contoso.com: CalendarSharingFreeBusySimple', 'atlanta.contoso.com: CalendarSharingFreeBusyReviewer', 'beijing.contoso.com: CalendarSharingFreeBusyReviewer'
    ```

- This example sets the sharing policy Contoso as the default sharing policy.

    ```powershell
    Set-SharingPolicy -Identity Contoso -Default $True
    ```

- This example disables the sharing policy Contoso.

    ```powershell
    Set-SharingPolicy -Identity "Contoso" -Enabled $False
    ```

- The first example removes the sharing policy Contoso. The second example removes the sharing policy Contoso and suppresses the confirmation that you want to remove the policy.

  ```powershell
  Remove-SharingPolicy -Identity Contoso
  ```

  ```powershell
  Remove-SharingPolicy -Identity Contoso -Confirm
  ```

For detailed syntax and parameter information, see [Set-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-SharingPolicy) and [Remove-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/Remove-SharingPolicy).
