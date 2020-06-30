---
localization_priority: Normal
ms.author: v-mapenn
f1.keywords:
- NOCSH
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: bba2059a-7242-41d0-bb3f-baaf7ec1abd7
ms.reviewer: 
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid: MET150
description: Learn how to block Basic auth for client authentication in Exchange Online
audience: Admin
title: Disable Basic authentication in Exchange Online

---

# Disable Basic authentication in Exchange Online

> [!NOTE]
> If you've enabled _security defaults_ in your organization, Basic authentication is already disabled in Exchange Online. For more information, see [What are security defaults?](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-security-defaults).

Basic authentication in Exchange Online uses a username and a password for client access requests. Blocking Basic authentication can help protect your Exchange Online organization from brute force or password spray attacks. When you disable Basic authentication for users in Exchange Online, their email clients and apps must support modern authentication. Those clients are:

- Outlook 2013 or later (Outlook 2013 requires a registry key change. See [Enable Modern Authentication for Office 2013 on Windows devices](https://docs.microsoft.com/microsoft-365/admin/security-and-compliance/enable-modern-authentication) for more information.)

- Outlook 2016 for Mac or later

- Outlook for iOS and Android

- Mail for iOS 11.3.1 or later

If your organization has no legacy email clients, you can use authentication policies in Exchange Online to disable Basic authentication requests, which forces all client access requests to use modern authentication. For more information about modern authentication, see [Using modern authentication with Office clients](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016).

This topic explains how Basic authentication is used and blocked in Exchange Online, and the corresponding procedures for authentication policies.

## How Basic authentication works in Exchange Online

Basic authentication is also known as _proxy authentication_ because the email client transmits the username and password to Exchange Online, and Exchange Online forwards or _proxies_ the credentials to an authoritative identity provider (IdP) on behalf of the email client or app. The IdP depends your organization's authentication model:

- **Cloud authentication**: The IdP is Azure Active Directory.

- **Federated authentication**: The IdP is an on-premises solution like Active Directory Federation Services (AD FS).

These authentication models are described in the following sections. For more information, see [Choose the right authentication method for your Azure Active Directory hybrid identity solution](https://docs.microsoft.com/azure/active-directory/hybrid/choose-ad-authn).

### Cloud authentication

The steps in cloud authentication are described in the following diagram:

![Basic steps for cloud-based authentication, and where Basic authentication is blocked.](../media/c2dcdb91-e149-4922-85a2-ad5f73b49b22.png)

1. The email client sends the username and password to Exchange Online.

    **Note**: When Basic authentication is blocked, it's blocked at this step.

2. Exchange Online sends the username and password to Azure Active Directory.

3. Azure Active Directory returns a user ticket to Exchange Online and the user is authenticated.

### Federated authentication

The steps in federated authentication are described in the following diagram:

![Basic steps for federated authentication, and where Basic authentication is blocked](../media/fd76db16-4e28-4720-81dd-faaabbf2f30a.png)

1. The email client sends the username and password to Exchange Online.

    **Note**: When Basic authentication is blocked, it's blocked at this step.

2. Exchange Online sends the username and password to the on-premises IdP.

3. Exchange Online receives a Security Assertion Markup Language (SAML) token from the on-premises IdP.

4. Exchange Online sends the SAML token to Azure Active Directory.

5. Azure Active Directory returns a user ticket to Exchange Online and the user is authenticated.

## How Basic authentication is blocked in Exchange Online

You block Basic authentication in Exchange Online by creating and assigning authentication policies to individual users. The policies define the client protocols where Basic authentication is blocked, and assigning the policy to one or more users blocks their Basic authentication requests for the specified protocols.

When it's blocked, Basic authentication in Exchange Online is blocked at the first pre-authentication step (Step 1 in the previous diagrams) before the request reaches Azure Active Directory or the on-premises IdP. The benefit of this approach is brute force or password spray attacks won't reach the IdP (which might trigger account lock-outs due to incorrect login attempts).

Because authentication policies operate at the user level, Exchange Online can only block Basic authentication requests for users that exist in the cloud organization. For federated authentication, if a user doesn't exist in Exchange Online, the username and password are forwarded to the on-premises IdP. For example, consider the following scenario:

1. An organization has the federated domain contoso.com and uses on-premises AD FS for authentication.

2. The user ian@contoso.com exists in the on-premises organization, but not in Office 365 or Microsoft 365 (there's no user account in Azure Active Directory and no recipient object in the Exchange Online global address list).

3. An email client sends a login request to Exchange Online with the username ian@contoso.com. An authentication policy can't be applied to the user, and the authentication request for ian@contoso.com is sent to the on-premises AD FS.

4. The on-premises AD FS can either accept or reject the authentication request for ian@contoso.com. If the request is accepted, a SAML token is returned to Exchange Online. As long as the SAML token's **ImmutableId** value matches a user in Azure Active Directory, Azure AD will issue a user ticket to Exchange Online (the **ImmutableId** value is set during Azure Active Directory Connect setup).

In this scenario, if contoso.com uses on-premises AD FS server for authentication, the on-premises AD FS server will still receive authentication requests for non-existent usernames from Exchange Online during a password spray attack.

In an Exchange hybrid deployment, authentication for your on-premises mailboxes will be handled by your on-premises Exchange servers, and authentication policies won't apply. For mailboxes moved to Exchange Online, the Autodiscover service will redirect them to Exchange Online, and then some of the previous scenarios will apply.

## Authentication policy procedures in Exchange Online

You manage all aspects of authentication policies in Exchange Online PowerShell. The protocols and services in Exchange Online that you can block Basic authentication for are described in the following table.

|**Protocol or service**|**Description**|**Parameter name**|
|:-----|:-----|:-----|
|Exchange Active Sync (EAS)|Used by some email clients on mobile devices.|*AllowBasicAuthActiveSync*|
|Autodiscover|Used by Outlook and EAS clients to find and connect to mailboxes in Exchange Online|*AllowBasicAuthAutodiscover*|
|IMAP4|Used by IMAP email clients.|*AllowBasicAuthImap*|
|MAPI over HTTP (MAPI/HTTP)|Used by Outlook 2010 and later.|*AllowBasicAuthMapi*|
|Offline Address Book (OAB)|A copy of address list collections that are downloaded and used by Outlook.|*AllowBasicAuthOfflineAddressBook*|
|Outlook Service|Used by the Mail and Calendar app for Windows 10.|*AllowBasicAuthOutlookService*|
|POP3|Used by POP email clients.|*AllowBasicAuthPop*|
|Reporting Web Services|Used to retrieve report data in Exchange Online.|*AllowBasicAuthReportingWebServices*|
|Outlook Anywhere (RPC over HTTP)|Used by Outlook 2016 and earlier.|*AllowBasicAuthRpc*|
|Authenticated SMTP|Used by POP and IMAP client's to send email messages.|*AllowBasicAuthSmtp*|
|Exchange Web Services (EWS)|A programming interface that's used by Outlook, Outlook for Mac, and third-party apps.|*AllowBasicAuthWebServices*|
|PowerShell|Used to connect to Exchange Online with remote PowerShell. If you block Basic authentication for Exchange Online PowerShell, you need to use the Exchange Online PowerShell Module to connect. For instructions, see [Connect to Exchange Online PowerShell using multi-factor authentication](https://docs.microsoft.com/powershell/exchange/mfa-connect-to-exchange-online-powershell).|*AllowBasicAuthPowerShell*|

Typically, when you block Basic authentication for a user, we recommend that you block Basic authentication for all protocols. However, you can use the *AllowBasicAuth\** parameters (switches) on the **New-AuthenticationPolicy** and **Set-AuthenticationPolicy** cmdlets to selectively allow or block Basic authentication for specific protocols.

For email clients and apps that don't support modern authentication, you need to allow Basic authentication for the protocols and services that they require. These protocols and services are described in the following table:

|**Client**|**Protocols and services**|
|:-----|:-----|
|Older EWS clients|• Autodiscover <br/>• EWS|
|Older ActiveSync clients|• Autodiscover <br/>• ActiveSync|
|POP clients|• POP3 <br/>• Authenticated SMTP|
|IMAP clients|• IMAP4 <br/>• Authenticated SMTP|
|Outlook 2010|• Autodiscover <br/>• MAPI over HTTP<br/>• Offline Address Book<br/>• Outlook Anywhere (RPC over HTTP)<br/>• Exchange Web Services (EWS)|

> [!NOTE]
> Blocking Basic authentication will block app passwords in Exchange Online. For more information about app passwords, see [Create an app password](https://support.microsoft.com/office/3e7c860f-bda4-4441-a618-b53953ee1183).

### What do you need to know before you begin?

- Verify that modern authentication is enabled in your Exchange Online organization (it's enabled by default). For more information, see [Enable or disable modern authentication for Outlook in Exchange Online](enable-or-disable-modern-authentication-in-exchange-online.md).

- Verify your email clients and apps support modern authentication (see the list at the beginning of the topic). Also, verify that your Outlook desktop clients are running the minimum required cumulative updates. For more information, see [Outlook Updates](https://docs.microsoft.com/officeupdates/outlook-updates-msi).

- To learn how to connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=396554).

### Create and apply authentication policies

The steps to create and apply authentication policies to block Basic authentication in Exchange Online are:

1. Create the authentication policy.

2. Assign the authentication policy to users.

3. Wait 24 hours for the policy to be applied to users, or force the policy to be immediately applied.

These steps are described in the following sections.

#### Step 1: Create the authentication policy

To create a policy that blocks Basic authentication for all available client protocols in Exchange Online (the recommended configuration), use the following syntax:

```PowerShell
New-AuthenticationPolicy -Name "<Descriptive Name>"
```

This example creates an authentication policy named Block Basic Auth.

```PowerShell
New-AuthenticationPolicy -Name "Block Basic Auth"
```

For detailed syntax and parameter information, see [New-AuthenticationPolicy](https://docs.microsoft.com/powershell/module/exchange/new-authenticationpolicy).

 **Notes**:

- You can't change the name of the policy after you create it (the *Name* parameter isn't available on the **Set-AuthenticationPolicy** cmdlet).

- To enable Basic authentication for specific protocols in the policy, see the [Modify authentication policies](#modify-authentication-policies) section later in this topic. The same protocol settings are available on the **New-AuthenticationPolicy** and **Set-AuthenticationPolicy** cmdlets, and the steps to enable Basic authentication for specific protocols are the same for both cmdlets.

#### Step 2: Assign the authentication policy to users

The methods that you can use to assign authentication policies to users are described in this section:

- **Individual user accounts**: Use the following syntax:

  ```PowerShell
  Set-User -Identity <UserIdentity> -AuthenticationPolicy <PolicyIdentity>
  ```

  This example assigns the policy named Block Basic Auth to the user account laura@contoso.com.

  ```PowerShell
  Set-User -Identity laura@contoso.com -AuthenticationPolicy "Block Basic Auth"
  ```

- **Filter user accounts by attributes**: This method requires that the user accounts all share a unique filterable attribute (for example, Title or Department) that you can use to identify the users. The syntax uses the following commands (two to identify the user accounts, and the other to apply the policy to those users):

   ```PowerShell
   $<VariableName1> = Get-User -ResultSize unlimited -Filter <Filter>
   $<VariableName2> = $<VariableName1>.MicrosoftOnlineServicesID
   $<VariableName2> | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Basic Auth"}
   ```

   This example assigns the policy named Block Basic Auth to all user accounts whose **Title** attribute contains the value "Sales Associate".

   ```PowerShell
   $SalesUsers = Get-User -ResultSize unlimited -Filter "(RecipientType -eq 'UserMailbox') -and (Title -like '*Sales Associate*')"
   $Sales = $SalesUsers.MicrosoftOnlineServicesID
   $Sales | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Basic Auth"}
   ```

- **Use a list of specific user accounts**: This method requires a text file to identify the user accounts. Values that don't contain spaces (for example, the Office 365 or Microsoft 365 work or school account) work best. The text file must contain one user account on each line like this:

  > akol@contoso.com <br/> tjohnston@contoso.com <br/> kakers@contoso.com

  The syntax uses the following two commands (one to identify the user accounts, and the other to apply the policy to those users):

  ```PowerShell
  $<VariableName> = Get-Content "<text file>"
  $<VariableName> | foreach {Set-User -Identity $_ -AuthenticationPolicy <PolicyIdentity>}
  ```

  This example assigns the policy named Block Basic Auth to the user accounts specified in the file C:\My Documents\BlockBasicAuth.txt.

  ```PowerShell
  $BBA = Get-Content "C:\My Documents\BlockBasicAuth.txt"
  $BBA | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Basic Auth"}
  ```

- **Filter on-premises Active Directory user accounts that are synchronized to Exchange Online**: For details, see the [Filter on-premises Active Directory user accounts that are synchronized to Exchange Online](#filter-on-premises-active-directory-user-accounts-that-are-synchronized-to-exchange-online) section in this topic.

> [!NOTE]
> To remove the policy assignment from users, use the value `$null` for the *AuthenticationPolicy* parameter on the **Set-User** cmdlet.

#### Step 3: (Optional) Immediately apply the authentication policy to users

By default, when you create or change the authentication policy assignment on users or update the policy, the changes take effect within 24 hours. If you want the policy to take effect within 30 minutes, use the following syntax:

```PowerShell
Set-User -Identity <UserIdentity> -STSRefreshTokensValidFrom $([System.DateTime]::UtcNow)
```

This example immediately applies the authentication policy to the user laura@contoso.com.

```PowerShell
Set-User -Identity laura@contoso.com -STSRefreshTokensValidFrom $([System.DateTime]::UtcNow)
```

This example immediately applies the authentication policy to multiple users that were previously identified by filterable attributes or a text file. This example works if you're still in the same PowerShell session and you haven't changed the variables you used to identify the users (you didn't use the same variable name afterwards for some other purpose). For example:

```PowerShell
$Sales | foreach {Set-User -Identity $_ -STSRefreshTokensValidFrom $([System.DateTime]::UtcNow)}
```

or

```PowerShell
$BBA | foreach {Set-User -Identity $_ -STSRefreshTokensValidFrom $([System.DateTime]::UtcNow)}
```

### View authentication policies

To view a summary list of the names of all existing authentication policies, run the following command:

```PowerShell
Get-AuthenticationPolicy | Format-Table Name -Auto
```

To view detailed information about a specific authentication policy, use this syntax:

```PowerShell
Get-AuthenticationPolicy -Identity <PolicyIdentity>
```

This example returns detailed information about the policy named Block Basic Auth.

```PowerShell
Get-AuthenticationPolicy -Identity "Block Basic Auth"
```

For detailed syntax and parameter information, see [Get-AuthenticationPolicy](https://docs.microsoft.com/powershell/module/exchange/get-authenticationpolicy).

### Modify authentication policies

By default, when you create a new authentication policy without specifying any protocols, Basic authentication is blocked for all client protocols in Exchange Online. In other words, the default value of the *AllowBasicAuth\** parameters (switches) is `False` for all protocols.

- To enable Basic authentication for a specific protocol that's disabled, specify the switch without a value.

- To disable Basic authentication for a specific protocol that's enabled, you can only use the value `:$false`.

You can use the **Get-AuthenticationPolicy** cmdlet to see the current status of the *AllowBasicAuth\** switches in the policy.

This example enables basic authentication for the POP3 protocol and disables basic authentication for the IMAP4 protocol in the existing authentication policy named Block Basic Auth.

```PowerShell
Set-AuthenticationPolicy -Identity "Block Basic Auth" -AllowBasicAuthPop -AllowBasicAuthImap:$false
```

For detailed syntax and parameter information, see [Set-AuthenticationPolicy](https://docs.microsoft.com/powershell/module/exchange/set-authenticationpolicy).

### Configure the default authentication policy

The default authentication policy is assigned to all users who don't already have a specific policy assigned to them. Note that the authentication policies assigned to users take precedence to the default policy. To configure the default authentication policy for the organization, use this syntax:

```PowerShell
Set-OrganizationConfig -DefaultAuthenticationPolicy <PolicyIdentity>
```

This example configures the authentication policy named Block Basic Auth as the default policy.

```PowerShell
Set-OrganizationConfig -DefaultAuthenticationPolicy "Block Basic Auth"
```

> [!NOTE]
> To remove the default authentication policy designation, use the value `$null` for the *DefaultAuthenticationPolicy* parameter.

### Remove authentication policies

To remove an existing authentication policy, use this syntax:

```PowerShell
Remove-AuthenticationPolicy -Identity <PolicyIdentity>
```

This example removes the policy named Test Auth Policy.

```PowerShell
Remove-AuthenticationPolicy -Identity "Test Auth Policy"
```

For detailed syntax and parameter information, see [Remove-AuthenticationPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-authenticationpolicy).

### How do you know that you've successfully disabled Basic authentication in Exchange Online?

To confirm that the authentication policy was applied to users:

1. Run the following command to find the distinguished name (DN) value of the authentication policy:

   ```PowerShell
   Get-AuthenticationPolicy | Format-List Name,DistinguishedName
   ```

2. Use the DN value of the authentication policy in the following command:

   ```PowerShell
   Get-User -Filter "AuthenticationPolicy -eq '<AuthPolicyDN>'"
   ```

   For example:

   ```PowerShell
   Get-User -Filter "AuthenticationPolicy -eq 'CN=Block Basic Auth,CN=Auth Policies,CN=Configuration,CN=contoso.onmicrosoft.com,CN=ConfigurationUnits,DC=NAMPR11B009,DC=PROD,DC=OUTLOOK,DC=COM'"
   ```

When an authentication policy blocks Basic authentication requests from a specific user for a specific protocol in Exchange Online, the response is `401 Unauthorized`. No additional information is returned to the client to avoid leaking any additional information about the blocked user. An example of the response looks like this:

```output
HTTP/1.1 401 Unauthorized
Server: Microsoft-IIS/10.0
request-id: 413ee498-f337-4b0d-8ad5-50d900eb1f72
X-CalculatedBETarget: DM5PR2101MB0886.namprd21.prod.outlook.com
X-BackEndHttpStatus: 401
Set-Cookie: MapiRouting=#################################################; path=/mapi/; secure; HttpOnly
X-ServerApplication: Exchange/15.20.0485.000
X-RequestId: {3146D993-9082-4D57-99ED-9E7D5EA4FA56}:8
X-ClientInfo: {B0DD130A-CDBF-4CFA-8041-3D73B4318010}:59
X-RequestType: Bind
X-DiagInfo: DM5PR2101MB0886
X-BEServer: DM5PR2101MB0886
X-Powered-By: ASP.NET
X-FEServer: MA1PR0101CA0031
WWW-Authenticate: Basic Realm="",Basic Realm=""
Date: Wed, 31 Jan 2018 05:15:08 GMT
Content-Length: 0
```

## Filter on-premises Active Directory user accounts that are synchronized to Exchange Online

This method uses one specific attribute as a filter for on-premises Active Directory group members that will be synchronized with Exchange Online. This method allows you to disable legacy protocols for specific groups without affecting the entire organization.

Throughout this example, we'll use the **Department** attribute, because it's a common attributes that identifies users based on their department and role. To see all Active Directory user extended properties, go to [Active Directory: Get-ADUser Default and Extended Properties](https://social.technet.microsoft.com/wiki/contents/articles/12037.active-directory-get-aduser-default-and-extended-properties.aspx).

### Step 1: Find the Active Directory users and set the Active Directory user attributes

#### Get the members of an Active Directory group

These steps require the Active Directory module for Windows PowerShell. To install this module on your PC, you need to download and install the [Remote Server Administration Tools (RSAT)](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Run the following command in Active Directory PowerShell to return all groups in Active Directory:

```PowerShell
Get-ADGroup -Filter * | select -Property Name
```

After you get the list of groups, you can query which users belong to those groups and create a list based on any of their attributes. We recommend using the **objectGuid** attribute because the value is unique for each user.

```PowerShell
Get-ADGroupMember -Identity "<GroupName>" | select -Property objectGuid
```

This example returns the **objectGuid** attribute value for the members of the group named Developers.

```PowerShell
Get-ADGroupMember -Identity "Developers" | select -Property objectGuid
```

#### Set the filterable user attribute

After you identify the Active Directory group that contains the users, you need to set the attribute value that will be synchronized with Exchange Online to filter users (and ultimately disable Basic authentication for them).

Use the following syntax in Active Directory PowerShell to configure the attribute value for the members of the group that you identified in the previous step. The first command identifies the group members based on their **objectGuid** attribute value. The second command assigns the **Department** attribute value to the group members.

```PowerShell
$variable1 = Get-ADGroupMember -Identity "<GroupName>" | select -ExpandProperty "objectGUID"; Foreach ($user in $variable1) {Set-ADUser -Identity $user.ToString() -Add@{Department="<DepartmentName>"}}
```

This example sets the **Department** attribute to the value "Developer" for users that belong to the group named "Developers".

```PowerShell
$variable1 = Get-ADGroupMember -Identity "Developers" | select -ExpandProperty "objectGUID"; Foreach ($user in $variable1) {Set-ADUser -Identity $user.ToString() -Add@{Department="Developer"}}
```

Use the following syntax in Active Directory PowerShell to verify the attribute was applied to the user accounts (now or in the past):

```PowerShell
Get-ADUser -Filter "Department -eq '<DepartmentName>'" -Properties Department
```

This example returns all user accounts with the value "Developer" for the **Department** attribute.

```PowerShell
Get-ADUser -Filter "Department -eq 'Developer'" -Properties Department
```

### Step 2: Disable legacy authentication in Exchange Online

> [!NOTE]
> The attribute values for on-premises users are synchronized to Exchange Online only for users that have a valid Exchange Online license. For more information, see [Add users individually or in bulk](https://docs.microsoft.com/microsoft-365/admin/add-users/add-user).

The Exchange Online PowerShell syntax uses the following commands (two to identify the user accounts, and the other to apply the policy to those users):

```PowerShell
$<VariableName1> = Get-User -ResultSize unlimited -Filter <Filter>
$<VariableName2> = $<VariableName1>.MicrosoftOnlineServicesID
$<VariableName2> | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Basic Auth"}
```

This example assigns the policy named Block Basic Auth to all synchronized user accounts whose **Department** attribute contains the value "Developer".

```PowerShell
$developerUsers = Get-User -ResultSize unlimited -Filter "(RecipientType -eq 'UserMailbox') -and (department -like '*developer*')"
$developers = $developerUsers.MicrosoftOnlineServicesID
$developers | foreach {Set-User -Identity $_ -AuthenticationPolicy "Block Basic Auth"}
```

If you connect to Exchange Online PowerShell in an Active Directory PowerShell session, you can use the following syntax to apply the policy to all members of an Active Directory group.

This example creates a new authentication policy named Marketing Policy that disables Basic authentication for members of the Active Directory group named Marketing Department for ActiveSync, POP3, authenticated SMTP, and IMAP4 clients.

> [!NOTE]
> A known limitation in Active Directory PowerShell prevents the **Get-AdGroupMember** cmdlet from returning more than 5000 results. Therefore, the following example only works for Active Directory groups that have less than 5000 members.

```PowerShell
New-AuthenticationPolicy -Name "Marketing Policy" -AllowBasicAuthActiveSync $false -AllowBasicAuthPop $false -AllowBasicAuthSmtp $false -AllowBasicAuthImap $false
$users = Get-ADGroupMember "Marketing Department"
foreach ($user in $users) {Set-User -Identity $user.SamAccountName -AuthenticationPolicy "Marketing Policy"}
```
