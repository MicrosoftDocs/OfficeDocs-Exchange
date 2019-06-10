---
localization_priority: Normal
description: 'Summary: How users in your Exchange 2016 or Exchange 2019 organization can quickly set up their Outlook for iOS and Android accounts using Basic authentication.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 013dbe8c-30de-4c9c-baa9-75081b9229e8
ms.date: 7/5/2018
title: Account setup in Outlook for iOS and Android using Basic authentication
ms.collection: exchange-server
ms.reviewer: smithre4
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Account setup in Outlook for iOS and Android using Basic authentication

Outlook for iOS and Android offers Exchange administrators the ability to "push" account configurations to their on-premises users who use Basic authentication with the ActiveSync protocol. This capability works with any Mobile Device Management (MDM) provider who uses the [Managed App Configuration](https://www.apple.com/business/resources/docs/Managing_Devices_and_Corporate_Data_on_iOS.pdf) channel for iOS or the [Android in the Enterprise](https://developer.android.com/work/managed-configurations) channel for Android.

For on-premises users enrolled in Microsoft Intune, you can deploy the account configuration settings using Intune in the Azure Portal.

Once an account configuration has been created and the user enrolls their device, Outlook for iOS and Android will detect that an account is "Found" and will then prompt the user to add the account. The only information the user needs to enter to complete the setup process is their password. Then, the user's mailbox content will load and the user can begin using the app.

The following images show an example of the end-user setup process after Outlook for iOS and Android has been configured in Intune in the Azure Portal.

![Account setup for Outlook for iOS and Android on-premises](../../media/77f0906c-0d62-48a4-9a93-534e29dae7e0.png)

## Create an app configuration policy for Outlook for iOS and Android using Microsoft Intune

If you're using Microsoft Intune as your mobile device management provider, the following steps will allow you to deploy account configuration settings for your on-premises mailboxes that leverage basic authentication with the ActiveSync protocol. Once the configuration is created, you can assign the settings to groups of users, as detailed in the next section, [Assign configuration settings](account-setup.md#assignconfig).

> [!NOTE]
> If users in your organization use both iOS and Android for Work devices, you'll need to create a separate app configuration policy for each platform.

1. Sign in to the Azure portal.

2. Select **More Services \> Monitoring + Management \> Intune**.

3. On the **Mobile apps** blade of the Manage list, select **App configuration policies**.

4. On the **App configuration policies** blade, choose **Add**.

5. On the **Add app configuration** blade, enter a **Name**, and optional **Description** for the app configuration settings.

6. For **Device enrollment type**, choose **Managed devices**.

7. For **Platform**, choose **iOS** or **Android**.

8. Choose **Associated apps**, and then, on the **Associated apps** blade, choose **Microsoft Outlook**.

    > [!NOTE]
    > If Outlook is not listed as an available app, then you must add it by following the instructions in [Add Android store apps to Microsoft Intune](https://docs.microsoft.com/en-us/intune/store-apps-android) and [How to add iOS store apps to Microsoft Intune](https://docs.microsoft.com/en-us/intune/store-apps-ios).

9. Click **OK** to return to the **Add app configuration** blade.

10. Choose **Configuration settings**. On the **Configuration settings** blade, define the key value pairs that will supply configurations for Outlook for iOS and Android. The key value pairs you enter are defined later in this article, in the section [Key value pairs](account-setup.md#kvp).

    > [!NOTE]
    > To enter the key value pairs, you have a choice between using the configuration designer or entering an XML property list.

11. When you're done, choose **OK**.

12. On the **Add app configuration** blade, choose **Add**.

The newly created configuration policy will be displayed on the **App configuration policies** blade.

## Assign configuration settings
<a name="assignconfig"> </a>

You assign the settings you created in the previous section to groups of users in Azure Active Directory. When a user has the Microsoft Outlook app installed, the app will be managed by the settings you have specified. To do this:

1. From the Intune blade, on the **Mobile apps** blade of the Manage list, choose **App configuration policies**.

2. From the list of app configuration policies, select the one you want to assign.

3. On the next blade, choose **Assignments**.

4. On the **Assignments** blade, select the Azure AD group to which you want to assign the app configuration policy, then choose **Select**, and then **OK**.

## Key value pairs
<a name="kvp"> </a>

When you create an app configuration policy in the Azure Portal or through your MDM provider, you will need the following key value pairs:

|**Key**|**Values**|
|:-----|:-----|
|com.microsoft.outlook.EmailProfile.EmailAccountName|This value specifies the display name email account as it will appear to users on their devices.  <br/> **Value type**: String  <br/> **Accepted values**: Display Name  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: Yes  <br/> **Example**: user  <br/> **Intune Token**<sup>*</sup>: {{username}}|
|com.microsoft.outlook.EmailProfile.EmailAddress|This value specifies the email address to be used for sending and receiving mail.  <br/> **Value type**: String  <br/> **Accepted values**: Email address  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: Yes  <br/> **Example**: user@companyname.com  <br/> **Intune Token**<sup>*</sup>: {{mail}}|
|com.microsoft.outlook.EmailProfile.EmailUPN|This value specifies the User Principal Name or username for the email profile that will be used to authenticate the account.  <br/> **Value type**: String  <br/> **Accepted values**: UPN Address or username  <br/> **Default if not specified**: \<blank\>  <br/>**Required**: Yes  <br/> **Example**: userupn@companyname.com  <br/> **Intune Token**<sup>*</sup>: {{userprincipalname}}|
|com.microsoft.outlook.EmailProfile.ServerAuthentication|This value specifies the authentication method for the user.  <br/> **Value type**: String  <br/> **Accepted values**: 'Username and Password'  <br/> **Default if not specified**: 'Username and Password'  <br/> **Required**: No  <br/> **Example**: 'Username and Password'|
|com.microsoft.outlook.EmailProfile.ServerHostName|This value specifies the host name of your Exchange server.  <br/> **Value type**: String  <br/> **Accepted values**: ActiveSync FQDN  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: Yes  <br/> **Example**: mail.companyname.com|
|com.microsoft.outlook.EmailProfile.AccountDomain|This value specifies the user's account domain.  <br/> **Value type**: String  <br/> **Accepted values**: Domain  <br/> **Default if not specified**: \<blank\>  <br/> **Required**: No  <br/>**Example**: companyname|
|com.microsoft.outlook.EmailProfile.AccountType|This value specifies the account type being configured based on the authentication model.  <br/> **Value type**: String  <br/> **Accepted values**: BasicAuth  <br/> **Default if not specified**: BasicAuth  <br/> **Required**: No  <br/> **Example**: BasicAuth|

 <sup>*</sup> Microsoft Intune users can use tokens that will expand to the correct value according to the MDM enrolled user. See [Add app configuration policies for managed iOS devices](https://docs.microsoft.com/intune/app-configuration-policies-use-ios) for more information.
