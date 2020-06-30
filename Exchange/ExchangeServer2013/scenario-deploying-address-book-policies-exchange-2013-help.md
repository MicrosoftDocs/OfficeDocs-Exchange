---
title: 'Scenario: Deploying address book policies: Exchange 2013 Help'
TOCTitle: 'Scenario: Deploying address book policies'
ms:assetid: 6ac3c87d-161f-447b-afb2-149ae7e3f1dc
ms:mtpsurl: https://technet.microsoft.com/library/JJ657455(v=EXCHG.150)
ms:contentKeyID: 49289287
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Scenario: Deploying address book policies

_**Applies to:** Exchange Server 2013_

## Deployment Scenarios

The following three scenarios describe possible deployment solutions for three different organization types. Although there are many more scenarios, the most popular ones are covered here. The address lists and global address lists (GALs) in these scenarios were created based on filters, such as Custom Attributes, that grouped the objects logically.

## Scenario 1: Two separate companies - one Exchange organization

This scenario is applicable to government agencies, divisions, or departments that share infrastructure, but no reporting chain and have no common employees. In addition, the divisions don't have any special security or privacy concerns. In this scenario, two address book policies (ABPs) are created where employees can only see members of the same organization when the view the GAL or look at membership of other distribution groups. In addition, no users will be members of distribution groups that span the entire organization.

![Address Book Policies with two separate companies](images/JJ657455.b4fc82da-a659-4ade-ba33-d55d90dcf204(EXCHG.150).gif "Address Book Policies with two separate companies")

The Contoso and Humungous Insurance ABPs were created using the following address lists, global address lists, room lists, and OABs, which were created using a recipient filter that grouped the objects with a filter such as Custom Attribute. Because the two companies are separate without any interaction between the two, there aren't any address lists in common.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>   </p></td>
<td><p>Contoso</p></td>
<td><p>Humungous Insurance</p></td>
</tr>
<tr class="even">
<td><p>Address Lists</p></td>
<td><p>AL_CON_Groups</p>
<p>AL_CON_Users</p>
<p>AL_CON_Contacts</p></td>
<td><p>AL_HI_Groups</p>
<p>AL_HI_Users</p>
<p>AL_HI_Contacts</p></td>
</tr>
<tr class="odd">
<td><p>Global address list</p></td>
<td><p>GAL_CON</p></td>
<td><p>GAL_HI</p></td>
</tr>
<tr class="even">
<td><p>Room address list</p></td>
<td><p>AL_CON_Rooms</p></td>
<td><p>AL_HI_Rooms</p></td>
</tr>
<tr class="odd">
<td><p>Offline address book (OAB)</p></td>
<td><p>OAB_CON</p></td>
<td><p>OAB_HI</p></td>
</tr>
</tbody>
</table>

## Scenario 2: Two companies sharing a CEO

In this scenario, Fabrikam and Tailspin Toys share the same Exchange organization and the same CEO. The CEO is the only common person between the two companies. This scenario requires three ABPs that have the following characteristics:

- The users in Tailspin Toys can only see Tailspin Toys users when they browse the GAL.

- The users in Fabrikam can only see Fabrikam users when they browse the GAL.

- In each company, there is a SeniorLeaders distribution group that includes the senior leaders of that company and the CEO.

- Users who look at the CEO's group membership will only see groups that belong to the user's company. They won't see groups not in their own company.

- Three ABPs are created: **Fab**, **Tail**, and **CEO**.

![Two Companies One CEO](images/JJ657455.c87a5654-d456-4688-acb2-0be15ba1cda6(EXCHG.150).gif "Two Companies One CEO")

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>   </p></td>
<td><p>Fabrikam</p></td>
<td><p>Tailspin Toys</p></td>
<td><p>CEO</p></td>
</tr>
<tr class="even">
<td><p>Address lists</p></td>
<td><p>AL_FAB_Users_DGs</p>
<p>AL_FAB_Contacts</p></td>
<td><p>AL_TAIL_Users_DGs</p>
<p>AL_TAIL_Contacts</p></td>
<td><p>AL_FAB_Users_DGs</p>
<p>AL_FAB_Contacts</p>
<p>AL_TAIL_Users_DGs</p>
<p>AL_TAIL_Contacts</p></td>
</tr>
<tr class="odd">
<td><p>Global address list</p></td>
<td><p>GAL_FAB</p></td>
<td><p>GAL_TAIL</p></td>
<td><p>Default GAL</p></td>
</tr>
<tr class="even">
<td><p>Room address list</p></td>
<td><p>AL_FAB_Rooms</p></td>
<td><p>AL_TAIL_Rooms</p></td>
<td><p>Default All Rooms</p></td>
</tr>
<tr class="odd">
<td><p>Offline address book (OAB)</p></td>
<td><p>OAB_FAB</p></td>
<td><p>OAB_TAIL</p></td>
<td><p>Default OAB</p></td>
</tr>
</tbody>
</table>

When the CEO is added to the distribution groups in each organization and falls within the scope of each company's ABP, then the CEO becomes visible to each company. The CEO can create distribution groups that span both companies and will be visible within each company's GAL, but members of the distribution group will only be able to view the members of the group that are within their own organization.

## Scenario 3: Education

This scenario is applicable to schools or universities where a division of class rooms is necessary to ensure the privacy of the students. The Education scenario has the following characteristics:

- Students in each class can only see other students in their class, their teacher, and the principal.

- Teachers can only students in their own classrooms.

- Teachers can see all other teachers and the principal.

- Distribution groups are created for each class's parents and the faculty.

![Address Book Policies Education Scenario](images/JJ657455.435f3b1a-9752-4c61-ab8a-80115c643d12(EXCHG.150).gif "Address Book Policies Education Scenario")

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>   </p></td>
<td><p>Students_ClassA</p></td>
<td><p>Teachers_ClassA</p></td>
<td><p>Principal</p></td>
</tr>
<tr class="even">
<td><p>Address Lists</p></td>
<td><p>AL_ClassAAL_Principal</p></td>
<td><p>AL_ClassAAL_AllTeachersAL_AllGroupsAL_Principal</p></td>
<td><p>AL_ClassA</p>
<p>AL_ClassB</p>
<p>AL_AllTeachers</p>
<p>AL_AllStudents</p>
<p>AL_AllGroups</p></td>
</tr>
<tr class="odd">
<td><p>Global address list</p></td>
<td><p>GAL_StudentsClassA</p></td>
<td><p>GAL_TeachersClassA</p></td>
<td><p>GAL_Everyone</p></td>
</tr>
<tr class="even">
<td><p>Room address list</p></td>
<td><p>AL_BlankRoom</p></td>
<td><p>AL_BlankRoom</p></td>
<td><p>Default All Rooms</p></td>
</tr>
<tr class="odd">
<td><p>Offline address book (OAB)</p></td>
<td><p>OAB_StudentsClassA</p></td>
<td><p>OAB_TeachersClassA</p></td>
<td><p>Default OAB</p></td>
</tr>
</tbody>
</table>

## Considerations and best practices

Consider the following when using ABPs in your organization:

- For ABPs to work correctly, the user mailbox to which you apply the ABP must be on an Exchange 2010 SP3 or an Exchange 2013 server.

- Don't run the Exchange 2010 Client Access server role on the global catalog server. Doing so results in Active Directory being used for Name Service Provider Interface (NSPI) instead of the Microsoft Exchange Address Book service. You can run Exchange 2013 server roles on a global catalog server and have ABPs work correctly, however we don't recommend installing Exchange on a domain controller.

- You can't use hierarchical address books (HABs) and ABPs simultaneously. To learn more, see [Hierarchical address books](https://docs.microsoft.com/exchange/address-books/hierarchical-address-books/hierarchical-address-books).

- Any user assigned an ABP should exist in their own GAL.

- If you allow client applications to access Active Directory directly through LDAP, they will bypass the logic built into ABPs. Because Outlook for Mac 2011 and Entourage 2008 use direct LDAP queries to access Active Directory, those client applications won't function properly with ABPs if a domain controller or global catalog server is specified or provided to them by the Autodiscover service. Outlook for Mac 2011 can use EWS or a local OAB to access directory information. However, if Outlook for Mac 2011 can directly access an LDAP service, it will attempt to do so.

- The GAL used in an ABP must, at a minimum, contain all of the address lists, including the room address list, defined and specified in an ABP. Don't create a GAL that contains fewer objects than any of the address lists in the same ABP.

- We recommend creating distribution groups that don't cross virtual organization boundaries. Creating distribution groups that contain members of multiple virtual organizations results in the following issues:

  - If group members request delivery or read receipts when sending mail to the distribution group, they'll be able to see the email addresses of the group members in other virtual organizations

  - If an encrypted message is sent to the distribution group and some group members don't have valid digital IDs, the sender will receive a warning message that includes the total number of members who don't have valid IDs and a list of their email addresses. However, if some of those members without valid digital IDs are in a different organization than the sender, the warning message will include the correct count but won't include the email addresses of the members in the other organization. As a result, the total count won't match the list of member addresses.

    For example, let's say a distribution group contains five members total from two organizations, Agency A and Agency B. Three group members are from Agency A, and one of those members has as invalid digital ID. The other two members are from Agency B, and both of them have invalid digital IDs. If a member from Agency A sends an encrypted message to the distribution group, that member will receive a warning message stating that there are a total of three recipients without valid digital IDs. However, only the email address for the recipient from Agency A will be listed in the warning message.

  - ABP's don't apply to the **Get-Group** cmdlets. Therefore, any user or process that is able to run **Get-Group** will see all members of any group they have access to.

    We recommend that you modify the group management settings of the OWA Options so users can't use Outlook Web App to manage groups. To prevent users from using OWA Options to manage groups, exclude the users from the MyDistributionGroupMembership RBAC role. For details, see [MyDistributionGroupMembership role](mydistributiongroupmembership-role-exchange-2013-help.md).

  - If you allow users to use Outlook or Outlook Web App to manage groups, the group owners must have full visibility to the group membership list.

- All ABPs must contain a room address list. However, if your organization doesn't use room address lists, you can create a default empty room address list.

- Deploying ABPs doesn't prevent users in one virtual organization from sending email to users in another virtual organization. If you want to prevent users from sending email across organizations, we recommend that you create a transport rule. For example, to create a transport rule that prevents Contoso users from receiving messages from Fabrikam users, but still allows Fabrikam's senior leadership team to send messages to Contoso users, run the following Shell command:

  ```powershell
  New-TransportRule -Name "StopFabrikamtoContosoMail" -FromMemberOf "AllFabrikamEmployees" -SentToMemberOf "AllContosoEmployees" -DeleteMessage -ExceptIfFrom seniorleadership@fabrikam.com
  ```

- If you want to enforce a feature similar to ABP in the Lync client, you can set the `msRTCSIP-GroupingID` attribute on specific user objects. For details, see [PartitionByOU Replaced with msRTCSIP-GroupingID](https://docs.microsoft.com/previous-versions/office/skype-server-2010/gg429725(v=ocs.14)) topic.

## General deployment steps

## Migrating from address list segmentation to ABPs

If your organization configured the Exchange 2007 address list segregation solution in place by using the instructions in the white paper [Configuring Virtual Organizations and Address List Segregation in Exchange 2007](https://docs.microsoft.com/previous-versions/office/exchange-server-2007-technical-articles/bb936719(v=exchg.80)), you should first migrate to Exchange Server 2010 using the steps outlined in [Migrate to Exchange Server 2010 Address Book Policies from Exchange Server 2007 Address List Segregation](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/hh529930(v=exchg.141)). This procedure will require some down-time for your organization and you will therefore need to plan accordingly.

## New deployment of ABPs

If your organization is deploying Exchange 2013 ABPs and hasn't used the Exchange 2007 address list segregation, you can use these instructions to deploy ABPs in your organization.

The steps in this section will walk you through Scenario 2: Two companies sharing a CEO. In this scenario, two companies (Fabrikam and Tailspin Toys) are separate but share a CEO and senior leadership team.

## Step 1: Install and configure the Address Book Policy Routing agent

If you're using ABPs, and you don't want users in separate virtual organizations to view each other's potentially private information, you can turn on the Address Book Policy Routing agent. The Address Book Policy Routing agent is a Transport agent that runs on the Mailbox server that controls how recipients are resolved in the organization. When the Address Book Policy Routing Agent is installed and configured, users that are assigned different GALs appear as external recipients in that they can't view external recipients' contact cards.

For detailed instructions, see [Install and configure the Address Book Policy Routing agent](install-and-configure-the-address-book-policy-routing-agent-exchange-2013-help.md).

## Step 2: Divide your virtual organizations

You'll need to develop a way to divide your organizations. We recommend using the CustomAttribute1-15 property on the mailboxes, contacts, and groups instead of the pre-canned conditional attributes such as Company, Department, or StateOrProvince to divide the virtual organizations for the following reasons:

- Not all recipient types of objects have precanned conditional attributes in Active Directory. For example, Distribution Group and Dynamic Distribution Group do not support company, department, or state attributes.

- Not all precanned conditional attributes are exposed in cmdlets for some recipients. For example, the *Company*, *department*, and *StateOrProvince* parameters are not available on the exposed in cmdlets for mail users, contacts, distribution groups, and mail-enabled public folders.

- Multiple cmdlets are required to segregate recipient when you use the pre-canned conditional attribute. For example, you need to run Set-User to tag *Company*, *Department*, *StateOrProvince* for a UserMailbox after you run **New-Mailbox** or **Set-Mailbox** cmdlets.

- The *CustomAttributeX* parameters are all exposed in the Set-\* cmdlet for each recipient type, we can complete all segregation for that type via single Set- cmdlet

- CustomAttributeX attributes are explicitly reserved for customization of an organization and are entirely under the control of the organization administrators.

Another best practice to consider implementing when segregating your organization is to use company identifiers in the names of distribution groups and dynamic distribution groups. Exchange has a Group Naming policy feature that will automatically add a suffix or prefix to the name of the distribution group based on many attributes of the user creating the distribution group including the creator of the distribution group's Company, StateorProvince, Title, and CustomAttribute1 to CustomAttribute15. The group naming policy is especially important if you are allowing users to create their own distribution groups. For more information, see [Create a distribution group naming policy](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-distribution-groups/create-group-naming-policy).

Group naming policies don't apply to dynamic distribution groups, so you will need to manually segregate them and manually apply a naming policy.

## Step 3: Create the address lists, GALs, and OABs

When you create the address lists and global address lists do not use "IncludedRecipient" and "ConditionalX" parameters, such as ConditionalCompany and ConditionalCustomAttribute5. You should use a recipient filter instead. You must use the Shell to create recipient filters. For more information about Recipient Filters, see [Recipient filtering on Edge Transport servers](recipient-filtering-on-edge-transport-servers-exchange-2013-help.md)

In creating the ABP, you will create multiple address lists based on how you want your users to view the address lists in Outlook or Outlook Web App. This organization has four address lists:

- AL\_FAB\_Users\_DGs

- AL\_FAB\_Contacts

- AL\_TAIL\_Users\_DGs

- AL\_TAIL\_Contacts

This example creates the address list AL\_TAIL\_Users\_DGs. The address list contains all users and distribution groups where CustomAttribute15 equals TAIL.

```powershell
New-AddressList -Name "AL_TAIL_Users_DGs" -RecipientFilter "((RecipientType -eq 'UserMailbox') -or (RecipientType -eq 'MailUniversalDistributionGroup') -or (RecipientType -eq 'DynamicDistributionGroup')) -and (CustomAttribute15 -eq 'TAIL')"
```

For more information about creating address lists by using recipient filters, see [Create an address list by using recipient filters](https://docs.microsoft.com/exchange/address-books/address-lists/use-recipient-filters-to-create-an-address-list).

In order to create an ABP, you have to provide a room address list. If your organization doesn't have resource mailboxes such as room or equipment mailboxes, we suggest that you create a blank room address list. The following example creates a blank room address list because there are no room mailboxes in the organization.

```powershell
New-AddressList -Name AL_BlankRoom -RecipientFilter "(Alias -ne `$null) -and ((RecipientDisplayType -eq 'ConferenceRoomMailbox') -or (RecipientDisplayType -eq 'SyncedConferenceRoomMailbox'))"
```

However, in this scenario, Fabrikam and Contoso both have room mailboxes. This example creates room list for Fabrikam by using a recipient filter where CustomAttribute15 equals FAB.

```powershell
New-AddressList -Name AL_FAB_Room -RecipientFilter "(Alias -ne `$null) -and (CustomAttribute15 -eq 'FAB') -and (RecipientDisplayType -eq 'ConferenceRoomMailbox') -or (RecipientDisplayType -eq 'SyncedConferenceRoomMailbox')"
```

The global address list used in an ABP must be a superset of the address lists. Do not create a GAL with fewer objects than exists in any or all of the address lists in the ABP. This example creates the global address list for Tailspin Toys that includes all of the recipients that exists in the address lists and room address list.

```powershell
New-GlobalAddressList -Name "GAL_TAIL" -RecipientFilter "(CustomAttribute15 -eq 'TAIL')"
```

For more information, see [Create a global address list](https://docs.microsoft.com/exchange/address-books/address-lists/create-global-address-list).

When you create the OAB you should include the appropriate GAL when providing the *AddressLists* parameter of New- or Set-OfflineAddressBook to ensure no entry is unexpectedly missed. Basically, you can customize the set of entries that a user will see or reduce the download size of the OAB by specifying a list of AddressLists in AddressLists of New/Set-OfflineAddressBook. However, if you want users to see the full set of GAL entries in OAB, make sure that you include the GAL in the AddressLists.

This example creates the OAB for Fabrikam named OAB\_FAB.

```powershell
New-OfflineAddressBook -Name "OAB_FAB" -AddressLists "GAL_FAB"
```

For more information, see [Create an offline address book](https://docs.microsoft.com/exchange/address-books/offline-address-books/create-offline-address-book).

## Step 4: Create the ABPs

After you've created all of the required objects you can then create the ABP. This example creates the ABP named ABP\_TAIL.

```powershell
New-AddressBookPolicy -Name "ABP_TAIL" -AddressLists "AL_TAIL_Users_DGs"," AL_TAIL_Contacts" -OfflineAddressBook "\OAB_TAIL" -GlobalAddressList "\GAL_TAIL" -RoomList "\AL_TAIL_Rooms"
```

For more information, see [Create an address book policy](https://docs.microsoft.com/exchange/address-books/address-book-policies/create-an-address-book-policy).

## Step 5: Assign the ABPs to the mailboxes

Assigning the ABP to the user is the last step in the process. ABPs take effect when a user's application connects to the Microsoft Exchange Address Book service on the Client Access server. If the user is already connected to Outlook or Outlook Web App when the ABP is applied to their account, they will need to close and restart the client application before they can see their new address lists and GAL.

This example assigns ABP\_FAB to all mailboxes where CustomAttribute15 equals "FAB".

```powershell
Get-Mailbox -resultsize unlimited | where {$_.CustomAttribute15 -eq "TAIL"} | Set-Mailbox -AddressBookPolicy "ABP_TAIL"
```

For more information, see [Assign an address book policy to mail users](https://docs.microsoft.com/exchange/address-books/address-book-policies/assign-an-address-book-policy-to-mail-users).
