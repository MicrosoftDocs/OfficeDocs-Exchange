---
localization_priority: Critical
monikerRange: exchserver-2016 || exchserver-2019
description: 'Summary: Learn how to prepare Active Directory for Exchange 2016 or Exchange 2019.'
ms.topic: get-started-article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f895e1ce-d766-4352-ac46-ec959c9954a9
ms.reviewer: 
title: Prepare Active Directory and domains for Exchange Server, Active Directory Exchange Server, Exchange Server Active Directory, Exchange 2019 Active Directory
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Prepare Active Directory and domains for Exchange Server

Exchange uses Active Directory to store information about mailboxes and the configuration of Exchange servers in the organization. Before you install Exchange Server 2016 or Exchange Server 2019 (even if you have earlier versions of Exchange installed in your organization), you need to prepare your Active Directory forest and its domains for the new version of Exchange. There are two ways to do this:

- **Let the Exchange Setup wizard do it for you**: If you don't have a large Active Directory deployment, and you don't have a separate team that manages Active Directory, we recommend using the Setup wizard. Your account needs to be a member of both the Schema Admins and Enterprise Admins security groups. For more information about how to use the Setup wizard, check out [Install Exchange Mailbox servers using the Setup wizard](deploy-new-installations/install-mailbox-role.md).

- **Follow the steps in this topic**: If you have a large Active Directory deployment, or if a separate team manages Active Directory, this topic is for you. Following the steps in this topic gives you much more control over each stage of preparation, and who can do each step. For example, Exchange administrators might not have the required permissions to extend the Active Directory schema.

For details on new schema classes and attributes that Exchange adds to Active Directory, including those made by Cumulative Updates (CUs), see [Active Directory schema changes in Exchange Server](active-directory/ad-schema-changes.md).

For details about what's happening when Active Directory is being prepared for Exchange, see [What changes in Active Directory when Exchange is installed?](active-directory/ad-changes.md).

 If you aren't familiar with Active Directory forests or domains, check out [Active Directory Domain Services Overview](https://go.microsoft.com/fwlink/p/?LinkId=399226).

## What do you need to know before you begin?

- Estimated time to complete: 10-15 minutes or more (not including Active Directory replication), depending on organization size and the number of child domains.

- The computer that you use for these procedures needs to meet the [system requirements](system-requirements.md) for Exchange.

- Verify that your Active Directory meets the requirements for Exchange:

  - **Exchange 2019**: [Exchange 2019 Network and directory servers](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019#network-and-directory-servers).

  - **Exchange 2016**: [Exchange 2016 Network and directory servers](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2016#network-and-directory-servers).

- If your organization has multiple Active Directory domains, we recommend the following approach:

  - Do these procedures in an Active Directory site that contains an Active Directory server from every domain.

  - Install the first Exchange server in an Active Directory site that contains a writeable global catalog server from every domain.

- The computer that you use for all procedures in this topic requires access to Setup.exe in the Exchange installation files:

    1. Download the latest version of Exchange. For more information, see [Updates for Exchange Server](../new-features/updates.md).

    2. In File Explorer, right-click on the Exchange ISO image file that you downloaded, and then select **Mount**. Note the virtual DVD drive letter that's assigned.

    3. Open a Windows Command Prompt window. For example:

    - Press the Windows key + 'R' to open the **Run** dialog, type cmd.exe, and then press **OK**.

    - Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Step 1: Extend the Active Directory schema

> [!TIP]
> If you don't have a separate team that manages your Active Directory schema, you can skip this step and go directly to [Step 2: Prepare Active Directory](#step-2-prepare-active-directory). If you don't extend the schema in this step, the _/PrepareAd_ command in Step 2 will automatically extend the schema for you. If you skip this step, the requirements will also apply to Step 2.

When you extend the Active Directory schema for Exchange, the following requirements apply:

- Your account needs to be a member of the Schema Admins and Enterprise Admins security groups. If you have multiple Active Directory forests, make sure you're logged into the right one.

- The computer needs to be a member of the same Active Directory domain and site as the schema master.

- If you use the _/DomainController:\<DomainControllerFQDN\>_ switch, you need to specify the domain controller that's the schema master.

- The only supported way to extend the schema for Exchange is to use Setup.exe with _/PrepareSchema_, _/PrepareAD_, or the Exchange Setup wizard. Other ways of extending the schema aren't supported.

To extend the schema for Exchange, run the following command in a Windows Command Prompt window:

```console
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareSchema
```

For example, if the Exchange installation files are available on drive E:, run the following command:

```console
E:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareSchema
```

> [!NOTE]
> When you run this command, a prerequisite check is performed that will tell you which requirements are missing.

After Setup finishes extending the schema, you'll need to wait while Active Directory replicates the changes to all of your domain controllers before you proceed. To check the progress of the replication, you can use the `repadmin` tool in Windows Server. For more information about how to use the `repadmin` tool, see [Repadmin](https://go.microsoft.com/fwlink/p/?LinkId=257879).

## Step 2: Prepare Active Directory

After the Active Directory schema has been extended, you can prepare other parts of Active Directory for Exchange. During this step, Exchange will create containers, objects, and other items in Active Directory to store information. The collection of the Exchange containers, objects, attributes, and so on, is called the *Exchange organization*.

When you prepare Active Directory for Exchange, the following requirements apply:

- Your account needs to be a member of the Enterprise Admins security group. If you skipped Step 1 because you want the _/PrepareAD_ command to extend the schema, the account also needs to be a member of the Schema Admins security group.

- The computer needs to be a member of the same Active Directory domain and site as the schema master, and must be able to contact all of the domains in the forest on TCP port 389.

- Wait until Active Directory has finished replicating the schema changes from Step 1 to all domain controllers before you try to prepare Active Directory.

- You need to select a name for the Exchange organization. The organization name is used internally by Exchange, isn't typically seen by users, doesn't affect the functionality of Exchange, and doesn't determine what you can use for email addresses.

  - The organization name can't contain more than 64 characters, and can't be blank.

  - Valid characters are A to Z, a to z, 0 to 9, hyphen or dash (-), and space, but leading or trailing spaces aren't allowed.

  - You can't change the organization name after it's set.

To prepare Active Directory for Exchange, run the following command in a Windows Command Prompt window:

```console
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAD  /OrganizationName:"<Organization name>"
```

This example uses the Exchange installation files on drive E: and names the Exchange organization "Contoso Corporation".

```console
E:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAD /OrganizationName:"Contoso Corporation"
```

> [!IMPORTANT]
> If you have a hybrid deployment configured between your on-premises organization and Exchange Online, add the _/TenantOrganizationConfig_ switch to the command.

As in Step 1, you'll need to wait while Active Directory replicates the changes from this step to all of your domain controllers before you proceed, and you can use the `repadmin` tool to check the progress of the replication.

## Step 3: Prepare Active Directory domains

> [!TIP]
> If you have only one domain, you can skip this step because the _/PrepareAD_ command in Step 2 has already prepared the domain for you.

The final step is to prepare the Active Directory domain where Exchange servers will be installed or where mail-enabled users will be located. This step creates additional containers and security groups, and sets the permissions so Exchange can access them.

If you have multiple domains in your Active Directory forest, you have the following choices in how to prepare them:

- Prepare all domains in the Active Directory forest

- Choose the Active Directory domains to prepare

Regardless of the method you choose, wait until Active Directory has finished replicating the changes from Step 2 to all domain controllers before you proceed. Otherwise, you might get an error when you try to prepare the domains.

### Prepare all domains in the Active Directory forest

When you prepare all domains in the Active Directory forest for Exchange, your account needs to be a member of the Enterprise Admins security group.

To prepare all domains in your Active Directory forest, run the following command in a Windows Command Prompt window:

```console
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAllDomains
```

For example, if the Exchange installation files are available on drive E:, run the following command:

```console
E:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareAllDomains
```

### Choose the Active Directory domains to prepare

> [!TIP]
> You don't need to do this step in the domain where you ran the _/PrepareAD_ command in Step 2, because the _/PrepareAD_ command has automatically prepared that domain for you.

When you prepare specific domains in your Active Directory forest, the following requirements apply:

- You need to prepare every domain where an Exchange server will be installed.

- You need to prepare any domain that will contain mail-enabled users, even if the domain won't contain any Exchange servers.

- Your account needs to be a member of the Domain Admins group in the domain that you want to prepare.

- If the domain that you want to prepare was created **after** you ran /PrepareAD in Step 2, your account also needs to be a member of the Organization Management role group in Exchange.

To a prepare a specific domain in your Active Directory forest, run the following command in a Windows Command Prompt window:

```console
<Virtual DVD drive letter>:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareDomain[:<DomainFQDN>]
```

**Notes**:

- If the computer is a member of the domain that you want to prepare, you can use the _/PrepareDomain_ switch by itself. Otherwise, you need to specify the FQDN of the domain.

- You need to run this command for each Active Directory domain where you'll install an Exchange server or where mail-enabled users will be located.

This example uses the Exchange installation files on drive E: to prepare the engineering.corp.contoso.com domain:

```console
E:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareDomain:engineering.corp.contoso.com
```

This is the same example, but run on a computer that's a member of the engineering.corp.contoso.com domain:

```console
E:\Setup.exe /IAcceptExchangeServerLicenseTerms /PrepareDomain
```

## How do you know this worked?

To verify that you successfully prepared Active Directory and domains for Exchange, use any of the following steps:

- Use ADSI Edit and the information from the tables in the next section to verify that the specified objects have the correct values for the release of Exchange that you're installing. To learn more about ADSI Edit, see [ADSI Edit (adsiedit.msc)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc773354(v=ws.10)).

> [!CAUTION]
> Never change values in ADSI Edit unless you're told to do so by Microsoft Customer Service and Support. Changing values in ADSI Edit can cause irreparable damage to your Exchange organization and Active Directory.

- Check the Exchange setup log to verify that Active Directory preparation has completed successfully. For more information, see [Verify an Exchange installation](post-installation-tasks/verify-installation.md). Note that you can't use the **Get-ExchangeServer** cmdlet as described in the topic until you've completed the installation of at least one Exchange Mailbox server in an Active Directory site.

## Exchange Active Directory versions

The tables in the following sections contain the Exchange objects in Active Directory that are updated each time you install a new version of Exchange (a new installation or a CU). You can compare the object versions you see with the values in the tables to verify that Exchange successfully updated Active Directory during the installation.

- **rangeUpper** is located in the **Schema** naming context in the properties of the **ms-Exch-Schema-Version-Pt** container.

- **objectVersion (Default)** is the **objectVersion** attribute located in the **Default naming context** in the properties of the **Microsoft Exchange System Objects** container.

- **objectVersion (Configuration)** is the **objectVersion** attribute located in the **Configuration** naming context in **Services** \> **Microsoft Exchange** in the properties of the **\<Your Exchange Organization Name\>** container.

::: moniker range="exchserver-2019"
### Exchange 2019 Active Directory versions

|**Exchange 2019 version**|**rangeUpper**|**objectVersion (Default)**|**objectVersion (Configuration)**|
|:-----|:-----|:-----|:-----|
|Exchange 2019 CU6|17001|13237|16754|
|Exchange 2019 CU5|17001|13237|16754|
|Exchange 2019 CU4|17001|13237|16754|
|Exchange 2019 CU3|17001|13237|16754|
|Exchange 2019 CU2|17001|13237|16754|
|Exchange 2019 CU1|17000|13236|16752|
|Exchange 2019 RTM|17000|13236|16751|
|Exchange 2019 Preview|15332|13236|16213|
::: moniker-end

::: moniker range="exchserver-2016"
### Exchange 2016 Active Directory versions

|**Exchange 2016 version**|**rangeUpper**|**objectVersion (Default)**|**objectVersion (Configuration)**|
|:-----|:-----|:-----|:-----|
|Exchange 2016 CU17|15332|13237|16217|
|Exchange 2016 CU16|15332|13237|16217|
|Exchange 2016 CU15|15332|13237|16217|
|Exchange 2016 CU14|15332|13237|16217|
|Exchange 2016 CU13|15332|13237|16217|
|Exchange 2016 CU12|15332|13236|16215|
|Exchange 2016 CU11|15332|13236|16214|
|Exchange 2016 CU10|15332|13236|16213|
|Exchange 2016 CU9|15332|13236|16213|
|Exchange 2016 CU8|15332|13236|16213|
|Exchange 2016 CU7|15332|13236|16213|
|Exchange 2016 CU6|15330|13236|16213|
|Exchange 2016 CU5|15326|13236|16213|
|Exchange 2016 CU4|15326|13236|16213|
|Exchange 2016 CU3|15326|13236|16212|
|Exchange 2016 CU2|15325|13236|16212|
|Exchange 2016 CU1|15323|13236|16211|
|Exchange 2016 RTM|15317|13236|16210|
|Exchange 2016 Preview|15317|13236|16041|
::: moniker-end
