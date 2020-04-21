---
localization_priority: Normal
description: 'Summary: How to enable Outlook for iOS and Android in your Exchange Online environment in a secure manner.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: dd886cdc-bfc1-42a4-8e67-66ae1d08af0f
title: Securing Outlook for iOS and Android in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars
ms.custom: seo-marvel-apr2020
---

# Securing Outlook for iOS and Android in Exchange Online

Outlook for iOS and Android provides users the fast, intuitive email and calendar experience that users expect from a modern mobile app, while being the only app to provide support for the best features of Office 365.

Protecting company or organizational data on users' mobile devices is extremely important. Begin by reviewing [Setting up Outlook for iOS and Android](#setting-up-outlook-for-ios-and-android), to ensure your users have all the required apps installed. After that, choose one of the following options to secure your devices and your organization's data:

1. **Recommended**: If your organization has an Enterprise Mobility + Security subscription, or has separately obtained licensing for Microsoft Intune and Azure Active Directory Premium, follow the steps in [Leveraging Enterprise Mobility + Security suite to protect corporate data with Outlook for iOS and Android](#leveraging-enterprise-mobility--security-suite-to-protect-corporate-data-with-outlook-for-ios-and-android) to protect corporate data with Outlook for iOS and Android.

2. If your organization doesn't have an Enterprise Mobility + Security subscription or licensing for Microsoft Intune and Azure Active Directory Premium, follow the steps in [Leveraging Mobile Device Management for Office 365](#leveraging-mobile-device-management-for-office-365), and use the Mobile Device Management (MDM) for Office 365 capabilities that are included in your Office 365 subscription.

3. Follow the steps in [Leveraging Exchange Online mobile device policies](#leveraging-exchange-online-mobile-device-policies) to implement basic Exchange mobile device mailbox and device access policies.

If, on the other hand, you don't want to use Outlook for iOS and Android in your organization, see [Blocking Outlook for iOS and Android](#blocking-outlook-for-ios-and-android).

> [!NOTE]
> See [Exchange Web Services (EWS) application policies](#exchange-web-services-ews-application-policies) later in this article if you'd rather implement an EWS application policy to manage mobile device access in your organization.

## Setting up Outlook for iOS and Android

For devices enrolled in a mobile device management (MDM) solution, users will utilize the MDM solution, like the Intune Company Portal, to install the required apps: Outlook for iOS and Android and Microsoft Authenticator.

For devices that are not enrolled in an MDM solution, users need to install:

- Outlook for iOS and Android via the Apple App Store or Google Play Store

- Microsoft Authenticator app via the Apple App Store or Google Play Store

- Intune Company Portal app via Apple App Store or Google Play Store

Once the app is installed, users can follow these steps to add their corporate email account and configure basic app settings:

- [Set up email account in Outlook for iOS mobile app](https://support.office.com/article/b2de2161-cc1d-49ef-9ef9-81acd1c8e234)

- [Set up email in the Outlook for Android app](https://support.office.com/article/886db551-8dfa-4fd5-b835-f8e532091872)

- [Optimizing the Outlook mobile app for your iOS or Android phone](https://support.office.com/article/de075b19-b73c-4d8a-841b-459982c7e890)

> [!IMPORTANT]
> To leverage app-based conditional access policies, the Microsoft Authenticator app must be installed on iOS devices. For Android devices, the Intune Company Portal app is required. For more information, see [App-based Conditional Access with Intune](https://docs.microsoft.com/intune/app-based-conditional-access-intune).

## Leveraging Enterprise Mobility + Security suite to protect corporate data with Outlook for iOS and Android

> [!IMPORTANT]
> The Allow/Block/Quarantine (ABQ) list provides no security guarantees (if a client spoofs the DeviceType header, it might be possible to bypass blocking for a particular device type). To securely restrict access to specific device types, we recommend that you configure conditional access policies. For more information, see [App-based conditional access with Intune](https://docs.microsoft.com/intune/app-based-conditional-access-intune).

The richest and broadest protection capabilities for Office 365 data are available when you subscribe to the Enterprise Mobility + Security suite, which includes Microsoft Intune and Azure Active Directory Premium features, such as conditional access. At a minimum, you will want to deploy a conditional access policy that only allows connectivity to Outlook for iOS and Android from mobile devices and an Intune app protection policy that ensures the corporate data is protected.

> [!NOTE]
> While the Enterprise Mobility + Security suite subscription includes both Microsoft Intune and Azure Active Directory Premium, customers can purchase Microsoft Intune licenses and Azure Active Directory Premium licenses separately. All users must be licensed in order to leverage the conditional access and Intune app protection policies that are discussed in this article.

### Block all email apps except Outlook for iOS and Android using conditional access

When an organization decides to standardize how users access Exchange data, using Outlook for iOS and Android as the only email app for end users, they can configure a conditional access policy that blocks other mobile access methods. To do this, you will need several conditional access policies, with each policy targeting all potential users. Details on creating these policies can be found in [Require app protection policy for cloud app access with Conditional Access](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

1. Follow "Step 1: Configure an Azure AD Conditional Access policy for Office 365" in [Scenario 1: Office 365 apps require approved apps with app protection policies](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access#scenario-1-office-365-apps-require-approved-apps-with-app-protection-policies), which allows Outlook for iOS and Android, but blocks OAuth capable Exchange ActiveSync clients from connecting to Exchange Online.

   > [!NOTE]
   > This policy ensures mobile users can access all Office endpoints using the applicable apps.

2. Follow "Step 2: Configure an Azure AD Conditional Access policy for Exchange Online with ActiveSync (EAS)" in [Scenario 1: Office 365 apps require approved apps with app protection policies](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access#scenario-1-office-365-apps-require-approved-apps-with-app-protection-policies), which prevents Exchange ActiveSync clients leveraging basic authentication from connecting to Exchange Online.

   The above policies leverage the grant control [Require app protection policy](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference), which ensures that an Intune App Protection Policy is applied to the associated account within Outlook for iOS and Android prior to granting access. If the user isn't assigned to an Intune App Protection Policy, isn't licensed for Intune, or the app isn't included in the Intune App Protection Policy, then the policy prevents the user from obtaining an access token and gaining access to messaging data.

3. Finally, follow [How to: Block legacy authentication to Azure AD with Conditional Access](https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication) to block legacy authentication for other Exchange protocols on iOS and Android devices; this policy should target only Office 365 Exchange Online cloud app and iOS and Android device platforms. This ensures mobile apps using Exchange Web Services, IMAP4, or POP3 protocols with basic authentication cannot connect to Exchange Online.

> [!NOTE]
> After the conditional access policies are enabled, it may take up to 6 hours for any previously connected mobile device to become blocked.
> 
> When the user authenticates in Outlook for iOS and Android, if there are any Azure Active Directory conditional access policies applied, then mobile device access rules (allow, block, or quarantine) in Exchange Online are skipped.
> 
> To leverage app-based conditional access policies, the Microsoft Authenticator app must be installed on iOS devices. For Android devices, the Intune Company Portal app is required. For more information, see [App-based Conditional Access with Intune](https://docs.microsoft.com/intune/app-based-conditional-access-intune).

### Protect corporate data in Outlook for iOS and Android using Intune app protection policies

App Protection Policies (APP) define which apps are allowed and the actions they can take with your organization's data. The choices available in APP enable organizations to tailor the protection to their specific needs. For some, it may not be obvious which policy settings are required to implement a complete scenario. To help organizations prioritize mobile client endpoint hardening, Microsoft has introduced taxonomy for its APP data protection framework for iOS and Android mobile app management. 

The APP data protection framework is organized into three distinct configuration levels, with each level building off the previous level: 

- **Enterprise basic data protection** (Level 1) ensures that apps are protected with a PIN and encrypted and performs selective wipe operations. For Android devices, this level validates Android device attestation. This is an entry level configuration that provides similar data protection control in Exchange Online mailbox policies and introduces IT and the user population to APP. 
- **Enterprise enhanced data protection** (Level 2) introduces APP data leakage prevention mechanisms and minimum OS requirements. This is the configuration that is applicable to most mobile users accessing work or school data. 
- **Enterprise high data protection** (Level 3) introduces advanced data protection mechanisms, enhanced PIN configuration, and APP Mobile Threat Defense. This configuration is desirable for users that are accessing high risk data. 

To see the specific recommendations for each configuration level and the minimum apps that must be protected, review [Data protection framework using app protection policies](https://docs.microsoft.com/mem/intune/apps/app-protection-framework). 

Regardless of whether the device is enrolled in an MDM solution, an Intune app protection policy needs to be created for both iOS and Android apps, using the steps in [How to create and assign app protection policies](https://docs.microsoft.com/intune/app-protection-policies). These policies, at a minimum, must meet the following conditions:

1. They include all Microsoft mobile applications, such as Edge, OneDrive, Office, or Teams, as this will ensure that users can access and manipulate work or school data within any Microsoft app in a secure fashion.

2. They are assigned to all users. This ensures that all users are protected, regardless of whether they use Outlook for iOS or Android.

3. Determine which framework level meets your requirements. Most organizations should implement the settings defined in **Enterprise enhanced data protection** (Level 2) as that enables data protection and access requirements controls. 

For more information on the available settings, see [Android app protection policy settings in Microsoft Intune](https://docs.microsoft.com/intune/app-protection-policy-settings-android) and [iOS app protection policy settings](https://docs.microsoft.com/intune/app-protection-policy-settings-ios).

> [!IMPORTANT]
> To apply Intune app protection policies against apps on Android devices that are not enrolled in Intune, the user must also install the Intune Company Portal. For more information, see [What to expect when your Android app is managed by app protection policies](https://docs.microsoft.com/intune/app-protection-enabled-apps-android).

## Leveraging Mobile Device Management for Office 365

If you don't plan to leverage the Enterprise Mobility + Security suite, you can use Mobile Device Management (MDM) for Office 365. This solution requires that mobile devices be enrolled. When a user attempts to access Exchange Online with a device that is not enrolled, the user is blocked from accessing the resource until they enroll the device.

Because this is a device management solution, there is no native capability to control which apps can be used even after a device is enrolled. If you want to limit access to Outlook for iOS and Android, you will need to obtain Azure Active Directory Premium licenses and leverage the conditional access policies discussed in [Block all email apps except Outlook for iOS and Android using conditional access](#block-all-email-apps-except-outlook-for-ios-and-android-using-conditional-access).

An Office 365 global admin must complete the following steps to activate and set up MDM for Office 365. See [Set up Mobile Device Management (MDM) in Office 365](https://support.office.com/article/dd892318-bc44-4eb1-af00-9db5430be3cd) for complete steps. In summary, these steps include:

1. Activating MDM for Office 365 by following steps in the Security & Compliance Center.

2. Setting up MDM for Office 365 by, for example, creating an APNs certificate to manage iOS devices, and by adding a Domain Name System (DNS) record for your domain to support Windows phones.

3. Creating device policies and apply them to groups of users. When you do this, your users will get an enrollment message on their device. And when they've completed enrollment, their devices will be restricted by the policies you've set up for them.

> [!NOTE]
> Policies and access rules created in MDM for Office 365 will override both Exchange mobile device mailbox policies and device access rules created in the Exchange admin center. After a device is enrolled in MDM for Office 365, any Exchange mobile device mailbox policy or device access rule that is applied to that device will be ignored.

## Leveraging Exchange Online mobile device policies

If you don't plan on leveraging either the Enterprise Mobility + Security suite or the MDM for Office 365 functionality, you can implement Exchange mobile device mailbox policy to secure the device, and device access rules to limit device connectivity.

### Mobile device mailbox policy

Outlook for iOS and Android supports the following mobile device mailbox policy settings in Exchange Online:

- Device encryption enabled

- Min password length

- Password enabled

For information on how to create or modify an existing mobile device mailbox policy, see [Mobile device mailbox policies in Exchange Online](../../clients-and-mobile-in-exchange-online/exchange-activesync/mobile-device-mailbox-policies.md).

In addition, Outlook for iOS and Android supports Exchange Online's device-wipe capability. With Outlook, a remote wipe only wipes data within the Outlook app itself and does not trigger a full device wipe. For more information on how to perform a remote wipe, see [Perform a remote wipe on a mobile phone](../mobile-access/remote-wipe-on-mobile-phone.md).


### Device access policy

Outlook for iOS and Android should be enabled by default, but in some existing Exchange Online environments the app may be blocked for a variety of reasons. Once an organization decides to standardize how users access Exchange data and use Outlook for iOS and Android as the only email app for end users, you can configure blocks for other email apps running on users' iOS and Android devices. You have two options for instituting these blocks within Exchange Online: the first option blocks all devices and only allows usage of Outlook for iOS and Android; the second option allows you to block individual devices from using the native Exchange ActiveSync apps.

> [!NOTE]
> Because device IDs are not governed by any *physical device* ID, they can change without notice. When this happens, it can cause unintended consequences when device IDs are used for managing user devices, as existing 'allowed' devices may be unexpectedly blocked or quarantined by Exchange. Therefore, we recommend administrators only set mobile device access policies that allow/block devices based on device type or device model.

#### Option 1: Block all email apps except Outlook for iOS and Android

You can define a default block rule and then configure an allow rule for Outlook for iOS and Android, and for Windows devices, using the following Exchange Online PowerShell commands. This configuration will prevent any Exchange ActiveSync native app from connecting, and will only allow Outlook for iOS and Android.

1. Create the default block rule:

   ```PowerShell
   Set-ActiveSyncOrganizationSettings -DefaultAccessLevel Block
   ```

2. Create an allow rule for Outlook for iOS and Android

   ```PowerShell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceModel -QueryString "Outlook for iOS and Android" -AccessLevel Allow
   ```

3. **Optional**: Create rules that allow Outlook on Windows devices for Exchange ActiveSync connectivity (WP refers to Windows Phone, WP8 refers to Windows Phone 8 and later, and WindowsMail refers to the Mail app included in Windows 10):

   ```PowerShell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "WP" -AccessLevel Allow
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "WP8" -AccessLevel Allow
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "WindowsMail" -AccessLevel Allow
   ```

#### Option 2: Block native Exchange ActiveSync apps on Android and iOS devices

Alternatively, you can block native Exchange ActiveSync apps on specific Android and iOS devices or other types of devices.

1. Confirm that there are no Exchange ActiveSync device access rules in place that block Outlook for iOS and Android:

   ```PowerShell
   Get-ActiveSyncDeviceAccessRule | Where-Object { $_.AccessLevel -eq "Block" -and $_.QueryString -like "Outlook*" } | Format-Table Name, AccessLevel, QueryString -AutoSize
   ```

   If any device access rules that block Outlook for iOS and Android are found, type the following to remove them:

   ```PowerShell
   Get-ActiveSyncDeviceAccessRule | Where-Object { $_.AccessLevel -eq "Block" -and $_.QueryString -like "Outlook*" } | Remove-ActiveSyncDeviceAccessRule
   ```

2. You can block most Android and iOS devices with the following commands:

   ```PowerShell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "Android" -AccessLevel Block
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "iPad" -AccessLevel Block
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "iPhone" -AccessLevel Block
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "iPod" -AccessLevel Block
   ```

3. Not all Android device manufacturers specify "Android" as the DeviceType. Manufacturers may specify a unique value with each release. In order to find other Android devices that are accessing your environment, execute the following command to generate a report of all devices that have an active Exchange ActiveSync partnership:

   ```PowerShell
   Get-MobileDevice | Select-Object DeviceOS,DeviceModel,DeviceType | Export-CSV c:\temp\easdevices.csv
   ```

4. Create additional block rules, depending on your results from Step 3. For example, if you find your environment has a high usage of HTCOne Android devices, you can create an Exchange ActiveSync device access rule that blocks that particular device, forcing the users to use Outlook for iOS and Android. In this example, you would type:

   ```PowerShell
   New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "HTCOne" -AccessLevel Block
   ```

   > [!NOTE]
   > The **-QueryString** parameter does not accept wildcards or partial matches.

**Additional resources**:

- [New-ActiveSyncDeviceAccessRule](https://docs.microsoft.com/powershell/module/exchange/devices/New-ActiveSyncDeviceAccessRule)

- [Get-MobileDevice](https://docs.microsoft.com/powershell/module/exchange/devices/Get-MobileDevice)

- [Set-ActiveSyncOrganizationSettings](https://docs.microsoft.com/powershell/module/exchange/devices/Set-ActiveSyncOrganizationSettings)

## Blocking Outlook for iOS and Android

If you don't want users in your organization to access Exchange data with Outlook for iOS and Android, the approach you take depends on whether you are using Azure Active Directory conditional access policies or Exchange Online's device access policies.

### Option 1: Block mobile device access using a conditional access policy

Azure Active Directory conditional access does not provide a mechanism whereby you can specifically block Outlook for iOS and Android while allowing other Exchange ActiveSync clients. With that said, conditional access policies can be used to block mobile device access in two ways:

- Option A: Block mobile device access on both the iOS and Android platforms

- Option B: Block mobile device access on a specific mobile device platform

#### Option A: Block mobile device access on both the iOS and Android platforms

If you want to prevent mobile device access for all users, or a subset of users, using conditional access, follow these steps.

Create conditional access policies, with each policy either targeting all users or a subset of users via a security group. Details are in [Azure Active Directory app-based conditional access](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#exchange-online-policy).

1. The first policy blocks Outlook for iOS and Android and other OAuth capable Exchange ActiveSync clients from connecting to Exchange Online. See "Step 1 - Configure an Azure AD conditional access policy for Exchange Online," but for the fifth step, choose **Block access**.

2. The second policy prevents Exchange ActiveSync clients leveraging basic authentication from connecting to Exchange Online. See "Step 2 - Configure an Azure AD conditional access policy for Exchange Online with ActiveSync (EAS)."

#### Option B: Block mobile device access on a specific mobile device platform

If you want to prevent a specific mobile device platform from connecting to Exchange Online, while allowing Outlook for iOS and Android to connect using that platform, create the following conditional access policies, with each policy targeting all users. Details are in [Azure Active Directory app-based conditional access](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#exchange-online-policy).

1. The first policy allows Outlook for iOS and Android on the specific mobile device platform and blocks other OAuth capable Exchange ActiveSync clients from connecting to Exchange Online. See "Step 1 - Configure an Azure AD conditional access policy for Exchange Online," but for step 4a, select only the desired mobile device platform (such as iOS) to which you want to allow access.

2. The second policy blocks the app on the specific mobile device platform and other OAuth capable Exchange ActiveSync clients from connecting to Exchange Online. See "Step 1 - Configure an Azure AD conditional access policy for Exchange Online," but for step 4a, select only the desired mobile device platform (such as Android) to which you want to block access, and for step 5, choose **Block access**.

3. The third policy prevents Exchange ActiveSync clients leveraging basic authentication from connecting to Exchange Online. See "Step 2 - Configure an Azure AD conditional access policy for Exchange Online with ActiveSync (EAS)."

### Option 2: Block Outlook for iOS and Android using Exchange mobile device access rules

If you are managing your mobile device access via Exchange Online's device access rules, you have two options:

- Option A: Block Outlook for iOS and Android on both the iOS and Android platforms

- Option B: Block Outlook for iOS and Android on a specific mobile device platform

Every Exchange organization has different policies regarding security and device management. If an organization decides that Outlook for iOS and Android doesn't meet their needs or is not the best solution for them, administrators have the ability to block the app. Once the app is blocked, mobile Exchange users in your organization can continue accessing their mailboxes by using the built-in mail applications on iOS and Android.

The `New-ActiveSyncDeviceAccessRule` cmdlet has a `Characteristic` parameter, and there are three `Characteristic` options that administrators can use to block the Outlook for iOS and Android app. The options are UserAgent, DeviceModel, and DeviceType. In the two blocking options described in the following sections, you will use one or more of these characteristic values to restrict the access that Outlook for iOS and Android has to the mailboxes in your organization.

The values for each characteristic are displayed in the following table:

|**Characteristic**|**String for iOS**|**String for Android**|
|:-----|:-----|:-----|
|DeviceModel|Outlook for iOS and Android|Outlook for iOS and Android|
|DeviceType|Outlook|Outlook|
|UserAgent|Outlook-iOS/2.0|Outlook-Android/2.0|

#### Option A: Block Outlook for iOS and Android on both the iOS and Android platforms

With the `New-ActiveSyncDeviceAccessRule` cmdlet, you can define a device access rule, using either the `DeviceModel` or `DeviceType` characteristic. In both cases, the access rule blocks Outlook for iOS and Android across all platforms, and will prevent any device, on both the iOS platform and Android platform, from accessing an Exchange mailbox via the app.

The following are two examples of a device access rule. The first example uses the `DeviceModel` characteristic; the second example uses the `DeviceType` characteristic.

```PowerShell
New-ActiveSyncDeviceAccessRule -Characteristic DeviceType -QueryString "Outlook" -AccessLevel Block
```

```PowerShell
New-ActiveSyncDeviceAccessRule -Characteristic DeviceModel -QueryString "Outlook for iOS and Android" -AccessLevel Block
```

#### Option B: Block Outlook for iOS and Android on a specific mobile device platform

With the `UserAgent` characteristic, you can define a device access rule that blocks Outlook for iOS and Android across a specific platform. This rule will prevent a device from using Outlook for iOS and Android to connect on the platform you specify. The following examples show how to use the device-specific value for the `UserAgent` characteristic.

To block Android and allow iOS:

```PowerShell
New-ActiveSyncDeviceAccessRule -Characteristic UserAgent -QueryString "Outlook-Android/2.0" -AccessLevel Block
New-ActiveSyncDeviceAccessRule -Characteristic UserAgent -QueryString "Outlook-iOS/2.0" -AccessLevel Allow
```

To block iOS and allow Android:

```PowerShell
New-ActiveSyncDeviceAccessRule -Characteristic UserAgent -QueryString "Outlook-Android/2.0" -AccessLevel Allow
New-ActiveSyncDeviceAccessRule -Characteristic UserAgent -QueryString "Outlook-iOS/2.0" -AccessLevel Block
```

## Exchange Online controls

Beyond Microsoft Intune, MDM for Office 365, and Exchange mobile device policies, you can manage the access that mobile devices have to information in your organization through various Exchange Online controls, as well as, whether to allow users access to add-ins within Outlook for iOS and Android.

### Exchange Web Services (EWS) application policies

An EWS application policy can control whether or not applications are allowed to leverage the REST API. Note that when you configure an EWS application policy that only allows specific applications access to your messaging environment, you must add the user-agent string for Outlook for iOS and Android to the EWS allow list.

The following example shows how to add the user-agent strings to the EWS allow list:

```PowerShell
Set-OrganizationConfig -EwsAllowList @{Add="Outlook-iOS/*","Outlook-Android/*"}
```

### Exchange User controls

With the native Microsoft sync technology, administrators can control usage of Outlook for iOS and Android at the mailbox level. By default, users are allowed to access mailbox data using Outlook for iOS and Android. The following example shows how to disable a user's mailbox access with Outlook for iOS and Android:

```PowerShell
Set-CASMailbox jane@contoso.com -OutlookMobileEnabled $false
```

### Managing add-ins

Outlook for iOS and Android lets users integrate popular apps and services with the email client. Add-ins for Outlook are available on the web, Windows, Mac, and mobile. Since add-ins are managed via Office 365, users are able to share data and messages between Outlook for iOS and Android and the unmanaged add-in (even when the account is managed by an Intune App Protection policy), unless add-ins are turned off for the user within the M365 admin center.

If you want to stop your end users from accessing and installing Outlook add-ins (which affects all Outlook clients), execute the following changes to roles in the M365 admin center:

- To prevent users from installing Office Store add-ins, remove the My Marketplace role from them.
- To prevent users from side loading add-ins, remove the My Custom Apps role from them.
- To prevent users from installing all add-ins, remove both, My Custom Apps and My Marketplace roles from them.

For more information, please see [Add-ins for Outlook](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/add-ins-for-outlook/add-ins-for-outlook) and how to [Manage deployment of Office 365 add-ins in the Microsoft 365 admin center](https://docs.microsoft.com/office365/admin/manage/manage-deployment-of-add-ins).
