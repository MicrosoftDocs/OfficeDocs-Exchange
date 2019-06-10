---
title: 'Recreating arbitration mailboxes: Exchange 2013 Help'
TOCTitle: Recreating arbitration mailboxes
ms:assetid: bb6b8524-aaee-4be8-a04e-e61cd2ab3465
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Mt829264(v=EXCHG.150)
ms:contentKeyID: 74518107
ms.date: 01/17/2018
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Recreating arbitration mailboxes

**Summary:** About arbitration mailboxes in Exchange 2013 and how to re-create them.

Exchange 2013 comes with five system mailboxes known as *arbitration mailboxes*. Arbitration mailboxes are used for storing different types of system data and for managing messaging approval workflow. The below chart lists each type of arbitration mailbox and their responsibilities.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Arbitration mailbox Name</th>
<th>Display name</th>
<th>Persisted capabilities</th>
<th>Function</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042</p></td>
<td><p>Microsoft Exchange Federation Mailbox</p></td>
<td><p>{}</p></td>
<td><p>This mailbox stores data used to maintain federation between different Exchange organizations. This includes Rights Management Services, cross-premises mail-flow monitoring probes and responses, notifications, online archives, messaging records management, and cross-premises free/busy information.</p></td>
</tr>
<tr class="even">
<td><p>SystemMailbox{1f05a927-9350-4efe-a823-5529c2d64109}</p></td>
<td><p>Microsoft Exchange Approval Assistant</p></td>
<td><p>{}</p></td>
<td><p>This mailbox is provisioned for use by the Exchange approval framework for recipient moderation and auto group approval requests.</p></td>
</tr>
<tr class="odd">
<td><p>Migration.8f3e7716-2011-43e4-96b1-aba62d229136</p></td>
<td><p>Microsoft Exchange Migration</p></td>
<td><p>{OrganizationCapabilityManagement}</p></td>
<td><p>Stores data for the Exchange migration service to use when moving mailboxes in batches.</p></td>
</tr>
<tr class="even">
<td><p>SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}</p></td>
<td><p>Microsoft Exchange</p></td>
<td><p>{OrganizationCapabilityUMDataStorage}</p></td>
<td><p>Discovery system mailbox.</p>
<p>Provisioned for use by e-Discovery feature, which is used by compliance officers to locate messages that match specified selection criteria. This mailbox is also used by Unified Messaging for storing UM console attending files and other information.</p></td>
</tr>
<tr class="odd">
<td><p>SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}</p></td>
<td><p>Microsoft Exchange</p></td>
<td><p>{OrganizationCapabilityUMGrammarReady, OrganizationCapabilityPstProvider, OrganizationCapabilityMessageTracking, OrganizationCapabilityMailRouting, OrganizationCapabilityClientExtensions, OrganizationCapabilityGMGen, OrganizationCapabilityOABGen, OrganizationCapabilityUMGrammar}</p></td>
<td><p>This is known as an organization mailbox. It is used for creating offline address books (OABs). To load-balance OAB generation across your organization, including across geographically separate sites, you can create additional organization mailboxes.</p></td>
</tr>
</tbody>
</table>

If you need to re-create one of more of these arbitration mailboxes, see the instructions that follow.

## What you should know before you begin

- Estimated time to complete: 10 minutes per procedure.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use Exchange Management Shell to re-create an arbitration mailbox

Use the following instructions to re-create a particular type of arbitration mailbox.

> [!NOTE]
> All steps in the following sections must be run from the same directory where you extracted the Exchange installation media.

## Re-create the Microsoft Exchange Federation Mailbox

To re-create the arbitration mailbox FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042:

1. If any arbitration mailboxes are missing, run the following command:

    ```powershell
    .\Setup /preparead /IAcceptExchangeServerLicenseTerms
    ```

2. In Exchange Management Shell, run the following:

    ```powershell
    Enable-Mailbox -Arbitration -Identity "FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042"
    ```

## Re-create the Microsoft Exchange Approval Assistant mailbox

To re-create the arbitration mailbox SystemMailbox{1f05a927-9350-4efe-a823-5529c2d64109}:

1. If any arbitration mailboxes are missing, run the following command:

    ```powershell
    .\Setup /preparead /IAcceptExchangeServerLicenseTerms
    ```

2. In Exchange Management Shell, run the following:

    ```powershell
    Get-User | Where-Object {$_.Name -like "SystemMailbox{1f05a927-7709-4e35-9dbe-d0f608fb781a}"} | Enable-Mailbox -Arbitration
    ```

## Re-create the Microsoft Exchange Migration mailbox

To re-create the arbitration mailbox Migration.8f3e7716-2011-43e4-96b1-aba62d229136:

1. If any arbitration mailboxes are missing, run the following command:

    ```powershell
    .\Setup /preparead /IAcceptExchangeServerLicenseTerms
    ```

2. In Exchange Management Shell, run the following:

    ```powershell
    Enable-Mailbox -Arbitration -Identity "Migration.8f3e7716-2011-43e4-96b1-aba62d229136"
    ```

3. In Exchange Management Shell, set the Persisted Capabilities (msExchCapabilityIdentifiers) by running the following command:

    ```powershell
    Set-Mailbox "Migration.8f3e7716-2011-43e4-96b1-aba62d229136" -Arbitration -Management:$True -Force
    ```

## Re-create the Microsoft Exchange Discovery system mailbox

To re-create the arbitration mailbox SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}:

1. Run the following command:

    ```powershell
    .\Setup /preparead /IAcceptExchangeServerLicenseTerms
    ```

## Re-create the Microsoft Exchange organization mailbox for OABs

To re-create the arbitration mailbox SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}:

1. If any arbitration mailboxes are missing, run the following command:

    ```powershell
    .\Setup /preparead /IAcceptExchangeServerLicenseTerms
    ```

2. In Exchange Management Shell, run the following:

    ```powershell
    Enable-Mailbox -Arbitration -Identity "SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}"
    ```

3. In Exchange Management Shell, set the Persisted Capabilities (msExchCapabilityIdentifiers) by running the following command:

    ```powershell
    Get-Mailbox "SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}" -Arbitration | Set-Mailbox -Arbitration -UMGrammar:$True -OABGen:$True -GMGen:$True -ClientExtensions:$True -MessageTracking:$True -PstProvider:$True -MaxSendSize 1GB -Force
    ```

When finished, if you run the command `$OABMBX = Get-Mailbox "SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}" -Arbitration (Get-ADUser $OABMBX.SamAccountName -Properties *).msExchCapabilityIdentifiers` you will see that 46, 47, and 51 are missing. Run the following command to add all of the capabilities back:

```powershell
Set-ADUser $OABMBX.SamAccountName -Add @{"msExchCapabilityIdentifiers"="40","42","43","44","47","51","52","46"}
```

## How do I know this worked?

To verify that you have successfully re-created the arbitration mailbox, use the **Get-Mailbox** cmdlet with the *Arbitration* switch to retrieve system mailboxes.

```powershell
Get-Mailbox -Arbitration | Format-Table Name, DisplayName
```

View the results of the command to verify that appropriate system mailbox, either by Name or Display Name from the above table, has been re-created.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
