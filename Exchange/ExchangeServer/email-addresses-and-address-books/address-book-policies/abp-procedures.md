---
localization_priority: Normal
description: 'Summary: Learn how to manage address book policies, how to assign address book policies to users, and how to install and enable the Address Book Policy Routing Agent in Exchange Server.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6359abaf-e6f6-4667-8c2b-3860728b39a9
ms.reviewer:
title: Procedures for address book policies in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Procedures for address book policies in Exchange Server

Address book policies (ABPs) allow you to segment users into specific groups to give them customized global address lists (GALs) in Outlook and Outlook on the web (formerly known as Outlook Web App). For more information about ABPs, see [Address book policies in Exchange Server](address-book-policies.md).

 **Note**: Implementing an ABP is a multi-step process that requires planning. For more information, see [Scenario: Deploying address book policies in Exchange Server](abp-scenarios.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: Less than 5 minutes.

- You can assign ABPs to mailboxes in the Exchange admin center (EAC), but all other ABP procedures require the Exchange Management Shell. For more information about accessing and using the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address book policies" entry in the [Email address and address book permissions](../../permissions/feature-permissions/address-book-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to view address book policies

To view ABPs, use this syntax:

```PowerShell
Get-AddressBookPolicy [-Identity <ABPIdentity>]
```

This example returns a summary list of all ABPs in the organization:

```PowerShell
Get-AddressBookPolicy
```

This example returns detailed information for the ABP named All Fabrikam ABP.

```PowerShell
Get-AddressBookPolicy -Identity "All Fabrikam ABP" | Format-List
```

For detailed syntax and parameter information, see [Get-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/get-addressbookpolicy).

## Use the Exchange Management Shell to create address book policies

An ABP requires one global address list (GAL), one offline address book (OAB), one room list, and one or more address lists. To view the available objects, use the **Get-GlobalAddressList**, **Get-OfflineAddressBook**, and **Get-AddressList** cmdlets.

 **Note**: The room list that's required for an ABP is an address list that specifies rooms (contains the filter `RecipientDisplayType -eq 'ConferenceRoomMailbox'`). It's not a room finder distribution group that you create with the _RoomList_ switch on the **New-DistributionGroup** or **Set-DistributionGroup** cmdlets.

To create an ABP, use this syntax:

```PowerShell
New-AddressBookPolicy -Name "<Unique Name>" -GlobalAddressList "<GAL>" -OfflineAddressBook "<OAB>" -RoomList "<RoomList>" -AddressLists "<AddressList1>","<AddressList2>"...
```

This example creates an ABP named All Fabrikam ABP with the these settings:

- **GAL**: All Fabrikam

- **OAB**: Fabrikam-All-OAB

- **Room list**: All Fabrikam Rooms

- **Address lists**: All Fabrikam Mailboxes, All Fabrikam DLs, and All Fabrikam Contacts

```PowerShell
New-AddressBookPolicy -Name "All Fabrikam ABP" -GlobalAddressList "\All Fabrikam" -OfflineAddressBook \Fabrikam-All-OAB -RoomList "\All Fabrikam Rooms" -AddressLists "\All Fabrikam Mailboxes","\All Fabrikam DLs","\All Fabrikam Contacts"
```

For detailed syntax and parameter information, see [New-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/new-addressbookpolicy).

### How do you know this worked?

To verify that you've successfully created an ABP, use either of these procedures:

- Run this command in the Exchange Management Shell to verify that the ABP is listed:

  ```PowerShell
  Get-AddressBookPolicy
  ```

- Replace _\<ABPIdentity\>_ with the name of the ABP, and run this command in the Exchange Management Shell to verify the property values:

  ```PowerShell
  Get-AddressBookPolicy -Identity "<ABPIdentity>" | Format-List
  ```

## Use the Exchange Management Shell to modify address book policies

You use the **Set-AddressBookPolicy** cmdlet to modify an existing ABP. The settings are identical to the settings that are available when you create an ABP.

- The _Name_, _GlobalAddressList_, _OfflineAddressBook_, and _RoomList_ parameters all take single values, so the value you specify replaces the existing value.

    This example modifies the ABP named "All Fabrikam ABP" by replacing the OAB with the specified OAB.

  ```PowerShell
  Set-AddressBookPolicy -Identity "All Fabrikam ABP" -OfflineAddressBook \Fabrikam-OAB-2
  ```

- The _AddressLists_ parameter takes multiple values, so you need to decide whether you want to *replace* the existing address lists in the ABP, or *add and remove* address lists without affecting the other address lists in the ABP.

    This example replaces the existing address lists in the ABP named Government Agency A with the specified address lists.

  ```PowerShell
  Set-AddressBookPolicy -Identity "Government Agency A" -AddressLists "GovernmentAgencyA-Atlanta","GovernmentAgencyA-Moscow"
  ```

    To add address lists to an ABP, you need to specify the new address lists *and* any existing address lists that you want to keep.

    This example adds the address list named Contoso-Chicago to the ABP named ABP Contoso, which is already configured to use the address list named Contoso-Seattle.

  ```PowerShell
  Set-AddressBookPolicy -Identity "ABP Contoso" -AddressLists "Contoso-Chicago","Contoso-Seattle"
  ```

    To remove address lists from an ABP, you need to specify the existing address lists that you want to keep, and omit the address lists that you want to remove.

    For example, the ABP named ABP Fabrikam uses the address lists named Fabrikam-HR and Fabrikam-Finance. To remove the Fabrikam-HR address list, specify only the Fabrikam-Finance address list.

  ```PowerShell
  Set-AddressBookPolicy -Identity "ABP Fabrikam" -AddressLists Fabrikam-Finance
  ```

For detailed syntax and parameter information, see [Set-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/set-addressbookpolicy).

### How do you know this worked?

To verify that you've successfully modify an ABP, replace _\<ABPIdentity\>_ with the name of the ABP, and run this command in the Exchange Management Shell to verify the property values:

```PowerShell
Get-AddressBookPolicy -Identity "<ABPIdentity>" | Format-List
```

## Use the Exchange Management Shell to remove address book policies

- You can't remove an ABP if it's assigned to a mailbox. To see if an ABP is assigned to a mailbox, replace _\<ABPIdentity\>_ with the name of the ABP, and run this command in the Exchange Management Shell to get the **DistinguishedName** value:

   `Get-AddressBookPolicy -Identity <ABPIdentity> | Format-List DistinguishedName`

    Then, use the **DistinguishedName** value of the ABP in this command to show all mailboxes where the ABP is assigned:

     `Get-Mailbox -ResultSize unlimited -Filter "AddressBookPolicy -eq '<DistinguishedName>'"`

- To remove ABP assignments from mailboxes, see the [Assign address book policies to mailboxes](#assign-address-book-policies-to-mailboxes) section in this topic.

To remove an ABP, use this syntax:

```PowerShell
Remove-AddressBookPolicy -Identity <ABPIdentity>
```

This example removes the ABP named ABP_TailspinToys.

```PowerShell
Remove-AddressBookPolicy -Identity "ABP_TailspinToys"
```

For detailed syntax and parameter information, see [Remove-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-addressbookpolicy).

### How do you know this worked?

To verify that you've successfully removed an ABP, use either of these procedures:

- Run this command in the Exchange Management Shell to verify that the ABP isn't listed:

  ```PowerShell
  Get-AddressBookPolicy
  ```

- Replace _\<ABPIdentity\>_ with the name of the ABP, and run this command to confirm that an error is returned:

  ```PowerShell
  Get-AddressBookPolicy -Identity "<ABPIdentity>"
  ```

## Assign address book policies to mailboxes

- Users aren't automatically assigned an ABP when you create mailboxes. If you don't assign an ABP to a mailbox, the GAL for your entire organization is visible to the user in Outlook and Outlook on the web.

- To identify your virtual organizations for ABPs, we recommend that you use the CustomAttribute1-15 attributes on mailboxes, contacts, and groups, because these attributes are the most widely available and manageable for all recipient types. For more information, see [Scenario: Deploying address book policies in Exchange Server](abp-scenarios.md).

- The procedures to assign ABPs to mailboxes or remove the ABP assignments from mailboxes are the same:

  - To assign ABPs to mailboxes, you select the ABP in EAC, or specify the ABP in the Exchange Management Shell.

  - To remove the ABP assignments from mailboxes, you select the value **[No Policy]** in the EAC, or use the value `$null` in the Exchange Management Shell.

### Use the Exchange admin center (EAC) to assign an ABP to a single mailbox

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, find the mailbox that you want to modify. You can:

   - Scroll through the list of mailboxes.

   - Click **Search** ![Search icon](../../media/ITPro_EAC_.png) and enter part of the user's name, email address, or alias.

   - Click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png) \> **Advanced search** to find the mailbox.

    Once you've found the mailbox that you want to modify, select it, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page that opens, click **Mailbox features**.

4. Click the drop-down arrow in **Address book policy**, and select the ADP that you want to apply.

   ![Address book policy settings for a mailbox in the EAC at Recipients \> select mailbox \> Edit \> Mailbox features](../../media/2b219961-4664-40b3-873c-5892f1fcf2b6.png)

    When you're finished, click **Save**.

 **Note**: You can also assign an ABP when you create a user mailbox in the EAC by clicking **More options**, and clicking the drop-down arrow in **Address book policy**.

### Use the Exchange Management Shell to assign an address book policy to a single mailbox

To assign an ABP to a mailbox, use this syntax:

```PowerShell
Set-Mailbox -Identity <MailboxIdentity> -AddressBookPolicy <ABPIdentity> or $null
```

This example assigns the ABP named All Fabrikam to mailbox joe@fabrikam.com.

```PowerShell
Set-Mailbox -Identity joe@fabrikam.com -AddressBookPolicy "All Fabrikam"
```

 **Note**: You can also assign an ABP when you create a user mailbox with the **New-Mailbox** cmdlet by using the _AddressBookPolicy_ parameter. If you don't specify an ABP when you create the mailbox, no ABP is assigned (the default value is blank or `$null`).

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

### Use the EAC to assign an address book policy to multiple mailboxes

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, find the mailboxes that you want to modify. For example:

   1. Click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png) \> **Advanced search**.

   2. In the **Advanced search** window that opens, select **Recipient types** and verify the default value **User mailbox**.

   3. Click **More options**, and then click **Add a condition**.

   4. In the **Select one** drop-down box that appears, select the appropriate **Custom attribute 1** to **Custom attribute 15** values that defines your virtual organizations.

   5. In the **Specify words or phrases** dialog that appears, enter the value that you want to search for, and then click **OK**.

   6. Back on the **Advanced search** window, click **OK**. In the EAC at **Recipients** \> **Mailboxes**, click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png) \> **Advanced search** to find user mailboxes.

3. In the list of mailboxes, select multiple mailboxes of the same type (for example, **User**) from the list. For example:

   - Select a mailbox, hold down the Shift key, and select another mailbox that's farther down in the list.

   - Hold down the CTRL key as you select each mailbox.

    After you select multiple mailboxes of the same type, the title of the details pane changes to **Bulk Edit**.

4. In the details pane, scroll down and click **More options**, scroll down to **Address Book Policy**, and then click **Update**.

   ![Bulk select mailboxes in the EAC to assign an address book policy](../../media/6319f0ec-686d-48e2-9061-2337e30116d5.png)

5. In the **Bulk assign address book policy** window that opens, select the ABP by clicking the drop-down arrow in **Select Address Book Policy**, and then click **Save**.

### Use the Exchange Management Shell to assign an address book policy to multiple mailboxes

You can use the **Get-Mailbox** or **Get-Content** cmdlets to identify the user mailboxes that you want to assign the ABP to. For example:

- Use the _Filter_ parameter to create OPATH filters that identify the mailboxes. For more information, see [Filterable Properties for the -Filter Parameter](https://docs.microsoft.com/powershell/exchange/filter-properties).

- Use a text file to specify the mailboxes. The text file contains one mailbox (email address, name, or other unique identifier) on each line like this:

  > ebrunner@tailspintoys.com <br/> fapodaca@tailspintoys.com <br/> glaureano@tailspintoys.com <br/> hrim@tailspintoys.com

This example assigns the ABP named ABP_EngineeringDepartment to all user mailboxes where the `CustomAttribute11` attribute contains the value Engineering Department.

```PowerShell
Get-Mailbox -Filter "RecipientType -eq 'UserMailbox' -and CustomAttribute11 -like '*Engineering Department'" | Set-Mailbox -AddressBookPolicy "ABP_EngineeringDepartment"
```

This example uses the text file C:\My Documents\Accounts.txt to assign the same ABP to the specified user mailboxes.

```PowerShell
Get-Content "C:\My Documents\Accounts.txt" | foreach {Set-Mailbox $_ -AddressBookPolicy "ABP_EngineeringDepartment"}
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).

### How do you know this worked?

To verify that you've successfully assigned an ABP to a mailbox, do any of these steps:

- In the EAC, go to **Recipients** \> **Mailboxes** \> select the mailbox \> click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mailbox features** and verify the **Address Book Policy** value.

  ![Address book policy settings for a mailbox in the EAC at Recipients \> select mailbox \> Edit \> Mailbox features](../../media/2b219961-4664-40b3-873c-5892f1fcf2b6.png)

- In the Exchange Management Shell, replace _\<MailboxIdentity\>_ with the identity of the mailbox (for example, name, alias, or email address), and run this command:

  ```PowerShell
  Get-Mailbox -Identity "<MailboxIdentity>" | Format-List AddressBookPolicy
  ```

- In the Exchange Management Shell, use the same filter that you used to identify the mailboxes. For example:

  ```PowerShell
  Get-Mailbox -Filter "RecipientType -eq 'UserMailbox' -and CustomAttribute11 -like '*Engineering Department'" | Format-Table -Auto Name,EmailAddress,AddressBookPolicy
  ```

- In the Exchange Management Shell, replace _\<ABPIdentity\>_ with the name of the ABP, and run this command to get the **DistinguishedName** value:

  ```PowerShell
  Get-AddressBookPolicy -Identity <ABPIdentity> | Format-List DistinguishedName
  ```

  Then, use the **DistinguishedName** value of the ABP in this command to show all mailboxes where the ABP is assigned:

  ```PowerShell
  Get-Mailbox -ResultSize unlimited -Filter "AddressBookPolicy -eq '<DistinguishedName>'"
  ```

## Use the Exchange Management Shell to install and configure the Address Book Policy Routing Agent

Address Book Policy routing (ABP routing) controls how recipients are resolved in organizations that use ABPs. When ABP routing is enabled, users that are assigned different GALs appear as external recipients to each other.

ABP routing requires that you install and enable the Address Book Policy Routing Agent (ABP Routing Agent) on all Mailbox servers in your organization, and enable ABP routing globally in your organization. After you do this, it might take up to 30 minutes for messages to be processed by the ABP Routing Agent.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport Agents" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

### Step 1: Install the ABP Routing agent

To install the ABP Routing Agent on the local Mailbox server, run this command on every Mailbox server in the organization.

```PowerShell
Install-TransportAgent -Name "ABP Routing Agent" -TransportAgentFactory "Microsoft.Exchange.Transport.Agent.AddressBookPolicyRoutingAgent.AddressBookPolicyRoutingAgentFactory" -AssemblyPath $env:ExchangeInstallPath\TransportRoles\agents\AddressBookPolicyRoutingAgent\Microsoft.Exchange.Transport.Agent.AddressBookPolicyRoutingAgent.dll
```

 **Note**: You'll get a warning that the Transport service needs to be restarted for the changes to take effect. But, don't restart the Transport service until you finish Step 2 (so you only have to restart the Transport service once).

For detailed syntax and parameter information, see [Install-TransportAgent](https://docs.microsoft.com/powershell/module/exchange/install-transportagent).

### Step 2: Enable the ABP Routing agent

To enable the ABP Routing Agent on the local Mailbox server, run this command on every Mailbox server in the organization.

```PowerShell
Enable-TransportAgent "ABP Routing Agent"
```

For detailed syntax and parameter information, see [Enable-TransportAgent](https://docs.microsoft.com/powershell/module/exchange/enable-transportagent).

### Step 3: Restart the Transport service

To restart the Transport service, run this command on every Mailbox server in the organization.

```PowerShell
Restart-Service MSExchangeTransport
```

For detailed syntax and parameter information, see [Get-TransportAgent](https://docs.microsoft.com/powershell/module/exchange/get-transportagent).

### Step 4: Enable ABP routing globally in the Exchange organization

To enable ABP routing globally in the Exchange organization, run this command once on any Mailbox server:

```PowerShell
Set-TransportConfig -AddressBookPolicyRoutingEnabled $true
```

For detailed syntax and parameter information, see [Set-TransportConfig](https://docs.microsoft.com/powershell/module/exchange/set-transportconfig).

 **Note**: To disable ABP routing after you've enabled it, do these steps:

1. Run this command once on any Mailbox server to globally disable ABP routing:

   ```PowerShell
   Set-TransportConfig -AddressBookPolicyRoutingEnabled $false
   ```

2. Disable the ABP Routing Agent by running this command on every Mailbox server where the agent is installed:

   ```PowerShell
   Disable-TransportAgent "ABP Routing Agent"
   ```

3. Run this command on every Mailbox server where the agent is installed:

   ```PowerShell
   Restart-Service MSExchangeTransport
   ```

### How do you know this worked?

To verify that you've successfully installed and configured the ABP Routing Agent, use any of these steps:

- Run this command on a Mailbox server to verify that ABP routing is enabled for the organization:

  ```PowerShell
  Get-TransportConfig | Format-List AddressBookPolicyRoutingEnabled
  ```

- Run this command on every Mailbox server to verify that the ABP Routing Agent is enabled:

  ```PowerShell
  Get-TransportAgent "ABP Routing Agent"
  ```

- Have a user that's assigned an ABP send an email message to an user that's assigned a different ABP, and verify that the sender's email address doesn't resolve to their display name.
