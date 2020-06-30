---
title: 'Exchange 2013 deployment permissions reference: Exchange 2013 Help'
TOCTitle: Exchange 2013 deployment permissions reference
ms:assetid: b13412d0-0cc4-4c1d-bf31-cae3d3e211a9
ms:mtpsurl: https://technet.microsoft.com/library/Ee681663(v=EXCHG.150)
ms:contentKeyID: 56348434
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 deployment permissions reference

_**Applies to:** Exchange Server 2013_

This topic describes the permissions that are required to set up a Microsoft Exchange Server 2013 organization. The universal security groups (USGs) that are associated with management role groups, and other Windows security groups and security principals, are added to the access control lists (ACLs) of various Active Directory objects. ACLs control what operations can be performed on each object. By understanding what permissions are granted to each role group, security group, or security principal, you can determine what minimum permissions are required to install Exchange 2013.

In some cases, the ACL isn't applied on the usual property, **ntSecurityDescriptor**, but on another property, such as **msExchMailboxSecurityDescriptor**. The directory service can't enforce security that isn't specified in the Windows security descriptor. In most cases, these ACLs are replicated to store ACLs on appropriate objects by the store service. Unfortunately, there is no tool to view these ACLs as anything other than raw binary data.

The columns of each permissions table include the following information:

  - **Account**: The security principal granted or denied the permissions.

  - **ACE type**: Access control entry (ACE) type

      - **Allow ACE**: An allow ACE allows the user or group associated with the ACE to access an item.

      - **Deny ACE**: A deny ACE prevents the user or group associated with the ACE from accessing an item.

  - **Inheritance**: The type of inheritance used for child objects.

      - **All** indicates that the permissions apply to the object and all sub-objects.

      - **Desc** indicates the permissions apply to the object class listed in the On Property/Applies To row.

      - **None** indicates those permissions only apply the object.

  - **Permissions**: The permissions granted to the account.

  - **On Property/Applies To**: In some cases, permissions apply only to a given property, property set, or object class. These limited permissions are specified here.

  - **Comments**: When applicable, this column explains why the permissions are required or provides other information about the permissions.

The permissions are generally listed in the table by the names that are used on the Active Directory Service Interfaces (ADSI) Edit (AdsiEdit.msc) **Security** property page in the **Advanced** view on the **View/Edit** tab. The ADSI Edit **Security** property page lists a much more condensed view of the permissions. The LDP tool (Ldp.exe) displays the access mask directly as a numeric value. The setup code refers to the permissions by predefined constants.

The following table shows the relationships between these values.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>ADSI Edit Summary page</th>
<th>ADSI Edit Advanced view, View/Edit tab</th>
<th>ACL entries applied to a given object</th>
<th>Binary value (access mask in LDP)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Full Control</p></td>
<td><p>Full Control</p></td>
<td><p><code>WRITE_OWNER | WRITE_DAC | READ_CONTROL | DELETE | ACTRL_DS_CONTROL_ACCESS | ACTRL_DS_LIST_OBJECT | ACTRL_DS_DELETE_TREE | ACTRL_DS_WRITE_PROP | ACTRL_DS_READ_PROP | ACTRL_DS_SELF | ACTRL_DS_LIST | ACTRL_DS_DELETE_CHILD | ACTRL_DS_CREATE_CHILD</code></p></td>
<td><p><code>0x000F01FF</code></p></td>
</tr>
<tr class="even">
<td><p>Read</p></td>
<td><p>List Contents + Read All Properties + Read Permissions</p></td>
<td><p><code>ACTRL_DS_LIST | ACTRL_DS_READ_PROP | READ_CONTROL</code></p></td>
<td><p><code>0x00020014</code></p></td>
</tr>
<tr class="odd">
<td><p>Write</p></td>
<td><p>Write All Properties + All Validated Writes</p></td>
<td><p><code>ACTRL_DS_WRITE_PROP | ACTRL_DS_SELF</code></p></td>
<td><p><code>0x00000028</code></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>List Contents</p></td>
<td><p><code>ACTRL_DS_LIST</code></p></td>
<td><p><code>0x00000004</code></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Read All Properties</p></td>
<td><p><code>ACTRL_DS_READ_PROP</code></p></td>
<td><p><code>0x00000010</code></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>Write All Properties</p></td>
<td><p><code>ACTRL_DS_WRITE_PROP</code></p></td>
<td><p><code>0x00000020</code></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Delete</p></td>
<td><p><code>DELETE</code></p></td>
<td><p><code>0x00010000</code></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>Delete Subtree</p></td>
<td><p><code>ACTRL_DS_DELETE_TREE</code></p></td>
<td><p><code>0x00000040</code></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Read Permissions</p></td>
<td><p><code>READ_CONTROL</code></p></td>
<td><p><code>0x00020000</code></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>Modify Permissions</p></td>
<td><p><code>WRITE_DAC</code></p></td>
<td><p><code>0x00040000</code></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>Modify Owner</p></td>
<td><p><code>WRITE_OWNER</code></p></td>
<td><p><code>0x00080000</code></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p>All Validated Writes</p></td>
<td><p><code>ACTRL_DS_SELF</code></p></td>
<td><p><code>0x00000008</code></p></td>
</tr>
<tr class="odd">
<td><p> </p></td>
<td><p>All Extended Rights</p></td>
<td><p><code>ACTRL_DS_CONTROL_ACCESS</code></p></td>
<td><p><code>0x00000100</code></p></td>
</tr>
<tr class="even">
<td><p>Create All Child Objects</p></td>
<td><p>Create All Child Objects</p></td>
<td><p><code>ACTRL_DS_CREATE_CHILD</code></p></td>
<td><p><code>0x00000001</code></p></td>
</tr>
<tr class="odd">
<td><p>Delete All Child Objects</p></td>
<td><p>Delete All Child Objects</p></td>
<td><p><code>ACTRL_DS_DELETE_CHILD</code></p></td>
<td><p><code>0x00000002</code></p></td>
</tr>
<tr class="even">
<td><p> </p></td>
<td><p> </p></td>
<td><p><code>ACTRL_DS_LIST_OBJECT</code></p></td>
<td><p><code>0x00000080</code></p></td>
</tr>
</tbody>
</table>

Extended rights are custom rights specified by individual applications. They are specified in the ACL. However, they are meaningless to Active Directory. The specific application enforces any extended rights. Examples of Exchange extended rights are "Create public folder" or "Create named properties in the information store."

For information about permissions that are set during a Microsoft Exchange Server 2010 installation, see [Exchange 2010 Deployment Permissions Reference](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ee681663(v=exchg.141)).

## Prepare Active Directory Permissions

The permissions tables in this section show the permissions set when you execute the `Setup /PrepareAD` command.

> [!NOTE]
> The permissions described in this section are the default permissions that are configured when you deploy Exchange 2013 using the shared permissions model. If you've deployed Exchange 2013 using the Active Directory split permissions model, the default permission are different. For more information on the changes to the default permissions when using Active Directory split permissions and the shared and split permissions models in general, see <A href="understanding-split-permissions-exchange-2013-help.md">Active Directory split permissions</A> in <A href="understanding-split-permissions-exchange-2013-help.md">Understanding split permissions</A>. If you don't choose to use Active Directory split permissions when you install Exchange, Exchange will use shared permissions.

## Microsoft Exchange Container Permissions

The following table shows the permissions that are set on the Microsoft Exchange container within the configuration partition.

### Distinguished name of the object: CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=\<domain\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Installation Account</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
<td><p>This is the account that is used to run <code>/PrepareAD</code>.</p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>Read Property</p>
<p>List Contents</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify Permissions</p></td>
<td><p>msExchSmtpRceiveConnector</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Delegated Setup</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Microsoft Exchange Autodiscover Container Permissions

The following table shows the permissions set on the Microsoft Exchange Autodiscover container within the configuration partition.

### Distinguished name of the object: CN=Microsoft Exchange Autodiscover,CN=Services,CN=Configuration,DC=\<domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Microsoft Exchange Organization Container Permissions

The permissions tables in this section show the permissions set on the Microsoft Exchange Organization and sub-containers within the configuration partition.

### Distinguished name of the object: CN=\<organization\>,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=\<domain\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account(s)</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Enterprise Admins</p>
<p>Root Domain Admins</p>
<p>Installation Account</p>
<p>Organization Management</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Send As</p>
<p>Receive As</p></td>
<td><p> </p></td>
<td><p>Windows administrators aren't allowed to open mailboxes.</p></td>
</tr>
<tr class="even">
<td><p>Enterprise Admins</p>
<p>Schema Admins</p>
<p>Root Domain Admins</p>
<p>Installation Account</p>
<p>Organization Management</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Exchange Web Services Impersonation</p>
<p>Exchange Web Services Token Serialization</p></td>
<td><p> </p></td>
<td><p>Extended right</p></td>
</tr>
<tr class="odd">
<td><p>Enterprise Admins</p>
<p>Schema Admins</p>
<p>Root Domain Admins</p>
<p>Installation Account</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Store Transport Access</p>
<p>Store Constrained Delegation</p>
<p>Store Read Access</p>
<p>Store Read Write Access</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Local System</p></td>
<td><p>Allow</p></td>
<td><p>All</p></td>
<td><p>All Extended Rights</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Deny ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p></td>
<td><p><code>msExchAvailabilityUserPassword / msExchAvailabilityAddressSpace</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow</p></td>
<td><p>None</p></td>
<td><p>Read</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>NT Authority\Network Service</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Managed Availability Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>All Extended Rights</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>groupType</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchOwningServer</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMailboxSecurityDescriptor</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMServerWritableFlags</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchDatabaseCreated</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUserCulture</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMobileMailboxFlags</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>siteFolderGUID</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>siteFolderServer</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchEDBOffline</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>userCertificate</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMDtmfMap</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchBlockedSendersHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Public Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPatchMDB</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>publicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMSpokenName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMPinChecksum</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>legacyExchangeDN</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchSafeSendersHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>thumbnailPhoto</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create top level public folder</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create top level public folder</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>View information store status</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>View information store status</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Administer information store</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Administer information store</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create named properties in the information store</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create named properties in the information store</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder ACL</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder ACL</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder quotas</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder quotas</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder admin ACL</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder admin ACL</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder expiry</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder expiry</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder replica list</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder replica list</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder deleted item retention</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Modify public folder deleted item retention</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create public folder</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create public folder</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Mail Enable Public Folder</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Everyone</p>
<p>NT Authority\Anonymous Logon</p>
<p></p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create named properties in the information store</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Everyone</p>
<p>NT Authority\Anonymous Logon</p>
<p></p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create public folder</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Everyone</p>
<p>NT Authority\Anonymous Logon</p>
<p></p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p><code>/ msExchPrivateMDB</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Everyone</p>
<p>NT Authority\Anonymous Logon</p>
<p></p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p><code>/ msExchPublicMDB</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p><code>/ siteAddressing</code></p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=All Address Lists,CN=Address Lists Container,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchLastAppliedRecipientFilter</code></p>
<p><code>msExchRecipientFilterFlags</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchLastAppliedRecipientFilter</code></p>
<p><code>msExchRecipientFilterFlags</code></p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Offline Address Lists,CN=Address Lists Container, CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Download Offline Address Book</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Addressing,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Recipient Policies,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchLastAppliedRecipientFilter</code></p>
<p><code>msExchRecipientFilterFlags</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchLastAppliedRecipientFilter</code></p>
<p><code>msExchRecipientFilterFlags</code></p></td>
</tr>
</tbody>
</table>

## Configuration Partition Container Permissions

The permissions tables in this section show the permissions set by the `Setup /PrepareAD` command on various containers within the configuration partition.

### Distinguished name of the object: CN=Sites,CN=Configuration,DC=\<domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchVersion / site</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchVersion / site-link</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPartnerId / site</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMinorPartnerId / site</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchResponsibleforSites / site</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p></p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchTransportSiteFlags / site</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchCost / site-link</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p>
<p>Local System</p>
<p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p><code>/ msExchEdgeSyncEHFConnector</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p>
<p>Local System</p>
<p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p><code>/ msExchEdgeSyncMservConnector</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Children</p></td>
<td><p>Create Child</p>
<p>Delete Child</p>
<p>Delete Tree</p></td>
<td><p><code>msExchEdgeSyncServiceConfig / site</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p>
<p>Local System</p>
<p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p><code>/ msExchEdgeSyncServiceConfig</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Children</p></td>
<td><p>Create Child</p>
<p>Delete Child</p>
<p>Delete Tree</p></td>
<td><p><code>msExchEdgeSyncMservConnector / msExchEdgeSyncServiceConfig</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p>
<p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Children</p></td>
<td><p>Create Child</p>
<p>Delete Child</p>
<p>Delete Tree</p></td>
<td><p><code>msExchEdgeSyncEHFConnector / msExchEdgeSyncServiceConfig</code></p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Deleted Objects,CN=Configuration,DC=\<domain\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Administration</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Installation Account</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permission</p>
<p>Write Permission</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p>This is the account that is used to run <code>/PrepareAD</code>.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Network Service</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Exchange Administrative Group Permissions

The `Setup /PrepareAD` command also configures the following permissions on the administrative groups within the organization.

### Distinguished name of the object: CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Access Recipient Update Service</p></td>
<td><p><code>msExchExchangeServer</code></p></td>
<td><p>Allows Exchange Recipient Administrators to stamp recipients with proxy address information.</p></td>
</tr>
<tr class="even">
<td><p>Local System</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Access Recipient Update Service</p></td>
<td><p><code>msExchExchangeServer</code></p></td>
<td><p>Allows the servers to stamp recipients with proxy address information.</p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Access Recipient Update Service</p></td>
<td><p><code>msExchExchangeServer</code></p></td>
<td><p>Allows Exchange Public Folder Administrators to stamp recipients with proxy address information.</p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Advanced Security Settings,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Encryption,CN=Advanced Security Settings,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>Read Property</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Arrays,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Database Availability Groups,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Databases,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Servers,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Receive As</p></td>
<td><p> </p></td>
<td><p>Exchange Servers aren't allowed to open mailboxes.</p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Microsoft Exchange Security Groups Container Permissions

The permissions tables in this section show the permissions set on the Microsoft Exchange Security Groups container within the root domain partition.

### Distinguished name of the object: OU=Microsoft Exchange Security Groups,DC=\<root domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p></td>
<td><p><code>/ Group</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Delete</p></td>
<td><p><code>/ group</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Write Property</p></td>
<td><p><code>Member / group</code></p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Organization Management,OU=Microsoft Exchange Security Groups,DC=\<root domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Public Folder Management,OU=Microsoft Exchange Security Groups,DC=\<root domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=ExchangeLegacyInterop,OU=Microsoft Exchange Security Groups,DC=\<root domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Exchange Servers,OU=Microsoft Exchange Security Groups,DC=\<root domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Root Domain Administrators</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Members</p>
<p>Write Members</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Child Domain Administrators</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Members</p>
<p>Write Members</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Prepare Domain

The following tables show the permissions set when you execute the `Setup /PrepareDomain` command.

> [!NOTE]
> The permissions described in this section are the default permissions that are configured when you deploy Exchange 2013 using the shared permissions model. If you've deployed Exchange 2013 using the Active Directory split permissions model, the default permission are different. For more information on the changes to the default permissions when using Active Directory split permissions and the shared and split permissions models in general, see <A href="understanding-split-permissions-exchange-2013-help.md">Active Directory split permissions</A> in <A href="understanding-split-permissions-exchange-2013-help.md">Understanding split permissions</A>. If you don't choose to use Active Directory split permissions when you install Exchange, Exchange will use shared permissions.

### Distinguished name of the object: DC=\<domain\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>NT AUTHORITY\NETWORK</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p>Grants Transport service read permissions.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>groupType</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMailboxSecurityDescriptor</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMServerWritableFlags</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUserCultulre</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>memberOf</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>userAccountControl</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>canonicalName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Replication Synchronization</p></td>
<td><p> </p></td>
<td><p>Extended right</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Chile</p>
<p>List Children</p></td>
<td><p><code>msExchActiveSyncDevices / User</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p>
<p>List Children</p></td>
<td><p><code>msExchActiveSyncDevices / inetOrgPerson</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchSafeSendersHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPublicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMobileMailboxFlags</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchSafeRecipientsHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>userCertificate</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMDtmfMap</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchBlockedSendersHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMSpokenName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMPinChecksum</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>thumbnailPhoto</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>legacyExchangeDN</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPublicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>textEncodedORAddress</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>proxyAddresses</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>mail</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayNamePrintable</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>showInAddressBook</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p><code>/ msExchDynamicDistributionList</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>adminDisplayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Public Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPublicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>adminDisplayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p><code>/ msExchDynamicDistributionList</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>textEncodedORAddress</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>showInAddressBook</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>legacyExchangeDN</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>proxyAddresses</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayNamePrintable</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>mail</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>pwdLastSet</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>WriteDACL</p></td>
<td><p><code>/ user</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>WriteDACL</p>
<p></p></td>
<td><p><code>/ inetOrgPerson</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Delete Tree</p></td>
<td><p><code>/ user</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Delete Tree</p>
<p></p></td>
<td><p><code>/ inetOrgPerson</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>sAMAccountName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete</p></td>
<td><p><code>/ contact</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete</p></td>
<td><p><code>/ inetOrgPerson</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete</p></td>
<td><p><code>/ user</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete</p></td>
<td><p><code>/ organizationUnit</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete</p></td>
<td><p><code>/ group</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p></td>
<td><p><code>/ computer</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Member</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>wwwHomePage</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>countryCode</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>userAccountControl</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>managedBy</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Reset Password on Next Logon</p></td>
<td><p> </p></td>
<td><p>Extended right</p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Change Password</p></td>
<td><p><code> / user</code></p></td>
<td><p>Extended right</p></td>
</tr>
<tr class="odd">
<td><p>Delegated Setup</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>User Account Restrictions</code></p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=AdminSDHolder,CN=System,DC=\<domain\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>NT AUTHORITY\NETWORK</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p>Grants Transport service read permissions.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>groupType</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMailboxSecurityDescriptor</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMServerWritableFlags</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUserCultulre</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>memberOf</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>userAccountControl</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>canonicalName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Replication Synchronization</p></td>
<td><p> </p></td>
<td><p>Extended right</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Chile</p>
<p>List Children</p></td>
<td><p><code>msExchActiveSyncDevices / User</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p>
<p>List Children</p></td>
<td><p><code>msExchActiveSyncDevices / inetOrgPerson</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchSafeSendersHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPublicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchMobileMailboxFlags</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchSafeRecipientsHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>userCertificate</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMDtmfMap</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchBlockedSendersHash</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMSpokenName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchUMPinChecksum</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>thumbnailPhoto</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>legacyExchangeDN</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPublicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>textEncodedORAddress</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>proxyAddresses</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>mail</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayNamePrintable</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>showInAddressBook</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p><code>/ msExchDynamicDistributionList</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>adminDisplayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Public Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchPublicDelegates</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>adminDisplayName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p><code>/ msExchDynamicDistributionList</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Exchange Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>textEncodedORAddress</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>showInAddressBook</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>legacyExchangeDN</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Personal Information</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>proxyAddresses</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>displayNamePrintable</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>mail</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>pwdLastSet</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>sAMAccountName</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>Member</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>wwwHomePage</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>countryCode</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>userAccountControl</code></p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Windows Permissions</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p><code>managedBy</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Delegated Setup</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>User Account Restrictions</code></p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Deleted Objects,DC=\<domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>List Contents</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Microsoft Exchange System Objects,DC=\<domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>NT AUTHORITY\NETWORK</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p>
<p>List Contents</p>
<p>Read Permissions</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>garbageCollPeriod</code></p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>adminDisplayName</code></p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p><code>modifyTimeStamp</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Delete Tree</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read PropertyDelete Tree</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p></p></td>
<td><p><code>/ msExchSystemMailbox</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p></td>
<td><p><code>/ publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p></p></td>
<td><p><code>/ user</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Delete Child</p>
<p></p></td>
<td><p><code>/ msExchSystemMailbox</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Delete Child</p>
<p></p></td>
<td><p><code>/ user</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Write Property</p></td>
<td><p><code>/ publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Write Property</p></td>
<td><p><code>/ msExchSystemMailbox</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Write Property</p></td>
<td><p><code>/ user</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Change Password</p>
<p>Reset Password on Next Logon</p></td>
<td><p><code>/ user</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Write Property</p></td>
<td><p><code>/ msExchSystemMailbox</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p></td>
<td><p><code>/ msExchSystemMailbox</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Write Property</p></td>
<td><p><code>/ user</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p></td>
<td><p><code>/ user</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>mail / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>displayNamePrintable / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>displayName / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>textEncodedORAddress / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>proxyAddresses / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>cn / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>showInAddressBook / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>Exchange Information / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p>legacyExchangeDN / publicFolder</p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>Exchange Personal Information / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msDSPhoneticDisplayName / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msExchPFContacts / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>garbageCollPeriod / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>name / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msExchPublicDelegates / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>mail / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>displayNamePrintable / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>displayName / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>textEncodedORAddress / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>proxyAddresses / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>cn / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>showInAddressBook / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>Exchange Information / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>legacyExchangeDN / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>Exchange Personal Information / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msDSPhoneticDisplayName / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msExchPFContacts / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>garbageCollPeriod / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>name / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Public Folder Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msExchPublicDelegates / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>mail / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>displayNamePrintable / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>displayName / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>textEncodedORAddress / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>proxyAddresses / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>cn / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>showInAddressBook / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>Exchange Information / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>legacyExchangeDN / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>Exchange Personal Information / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msDSPhoneticDisplayName / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msExchPFContacts / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>garbageCollPeriod / publicFolder</code></p></td>
</tr>
<tr class="odd">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>name / publicFolder</code></p></td>
</tr>
<tr class="even">
<td><p>Exchange Trusted Subsystem</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p>
<p>Write Property</p></td>
<td><p><code>msExchPublicDelegates / publicFolder</code></p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Exchange Install Domain Servers,CN=Microsoft Exchange System Objects,DC=\<domain\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Organization Management</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Server Role Installation

During installation of the Client Access and Mailbox server roles, Setup adds the Organization Management USG to the administrator security group on the local computer so that members of the management role group named Organization Management can manage the server.

The following permissions table shows the permissions set when you install the Client Access or Mailbox server roles.

### Distinguished name of the object: CN=\<server\>,CN=Servers,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>MACHINE$</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>MACHINE$</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>Write Property</p></td>
<td><p><code>msExchServerSite</code></p>
<p><code>msExchEdgeSyncCredential</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Store Transport Access</p>
<p>Store Constrained Delegation</p>
<p>Store Read Only Access</p>
<p>Store Read and Write Access</p></td>
<td><p> </p></td>
<td><p>Extended rights</p></td>
</tr>
<tr class="even">
<td><p>NT AUTHORITY\NETWORK</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Exchange Web Services Token Serialization</p></td>
<td><p> </p></td>
<td><p>Extended right</p>
<p>Only granted on Mailbox server role objects.</p></td>
</tr>
<tr class="odd">
<td><p>NT AUTHORITY\NETWORK</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Local System</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Permissions</p>
<p>List Contents</p>
<p>Read Property</p>
<p>List Object</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Delegated Setup</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Full Control</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Delegated Setup</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Create Child</p>
<p>Delete Child</p></td>
<td><p><code>/ msExchPublicMDB</code></p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Read Property</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Delegated Setup</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Receive As</p>
<p>Send As</p></td>
<td><p> </p></td>
<td><p>Extended right</p></td>
</tr>
</tbody>
</table>

## Database Availability Groups

The permissions tables in this section show the permissions set with regards to the database availability groups and its members.

### Distinguished name of the object: CN=\<DAGName\>,CN=Database Availability Groups,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>Read Property</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## Edge Transport

If you install an Edge Transport server and establish an Edge Subscription with the Exchange organization, the permissions in the following permissions table are set when the Edge Transport server is instantiated into the organization.

### Distinguished name of the object: CN=\<server\>,CN=Servers,CN=\<admin group\>,CN=Administrative Groups,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Write Property</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>None</p></td>
<td><p>Read Properties</p></td>
<td><p> </p></td>
<td><p>ACE is defined in schema for <code>msExchExchangeServer</code> class objects <code>defaultSecurityDescriptor</code>.</p></td>
</tr>
</tbody>
</table>

## Mailbox Server Installation

During installation of the first Mailbox server, the following containers are created, if they do not already exist. The following permissions table shows the permissions that are applied.

### Distinguished name of the object: CN=Availability Configuration,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>Desc</p></td>
<td><p>Read Property</p></td>
<td><p><code>msExchAvailabilityUserPassword / msExchAvailabilityAddressSpaceObjects</code></p></td>
<td><p>Extended right</p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Default \<Server\>,CN=SMTP Receive Connectors,CN=Protocols,CN=\<Server\>,CN=Servers,CN=\<admin group\>,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Deny ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known security identifier (SID) for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept EXCH50</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept EXCH50</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept EXCH50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept EXCH50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept EXCH50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XShadow</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XShadow</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSessionParams</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSessionParams</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSessionParams</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept xAttr</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept xAttr</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept xAttr</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XProxyFrom</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XProxyFrom</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XProxyFrom</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSysProbe</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XSysProbe</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XSysProbe</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Extended Properties</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Extended Properties</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Extended Properties</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Fast Index</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Fast Index</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Fast Index</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext AD Recipient Cache</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext AD Recipient Cache</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext AD Recipient Cache</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

### Distinguished name of the object: CN=Client \<Server\>,CN=SMTP Receive Connectors,CN=Protocols,CN=\<Server\>,CN=Servers,CN=\<admin group\>,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Authenticated Users</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSessionParams</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSessionParams</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSessionParams</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known security identifier (SID) for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Any Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Exch50</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to any Recipient</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XShadow</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XShadow</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>ExchangeLegacyInterop</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept xAttr</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept xAttr</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept xAttr</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XProxyFrom</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XProxyFrom</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XProxyFrom</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept XSysProbe</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XSysProbe</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Forest XSysProbe</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authentication Flag</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Anti-Spam</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Extended Properties</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Extended Properties</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Extended Properties</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Fast Index</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Fast Index</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext Fast Index</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Bypass Message Size Limit</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Organization Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext AD Recipient Cache</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext AD Recipient Cache</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XMessageContext AD Recipient Cache</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Submit Messages to Server</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Accept Authoritative Domain Sender</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p></p></td>
<td><p></p></td>
<td><p></p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>

## SMTP Send Connector Creation

The following table shows the permissions set when you create Send connectors.

### Distinguished name of the object: CN=\<Connector Name\>,CN=Connections,CN=\<routing group\>,CN=Routing Groups, CN=\<admin group\>,CN=\<organization\>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Account</th>
<th>ACE type</th>
<th>Inheritance</th>
<th>Permissions</th>
<th>On property/ Applies to</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>NT AUTHORITY\ANONYMOUS LOGON</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Organization Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Organization Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Organization Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Forest Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XShadow</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XShadow</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send XShadow</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-10</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for partner servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-24</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Routing Headers</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Legacy Exchange Servers.</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Servers</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Exch50</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-21</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Mailbox servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-22</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Edge Transport servers.</p></td>
</tr>
<tr class="even">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-23</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for externally secured servers.</p></td>
</tr>
<tr class="odd">
<td><p>S-1-9-1419165041-1139599005-3936102811-1022490595-24</p></td>
<td><p>Allow ACE</p></td>
<td><p>All</p></td>
<td><p>Send Exch50</p></td>
<td><p> </p></td>
<td><p>This is the well-known SID for Legacy Exchange Servers.</p></td>
</tr>
</tbody>
</table>
