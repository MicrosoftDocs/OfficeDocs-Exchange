---
title: 'View or configure Outlook Web App virtual directories: Exchange 2013 Help'
TOCTitle: View or configure Outlook Web App virtual directories
ms:assetid: 90babcf6-4486-4e01-9819-6d3ca4ed756c
ms:mtpsurl: https://technet.microsoft.com/library/Dd298140(v=EXCHG.150)
ms:contentKeyID: 49315461
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View or configure Outlook Web App virtual directories

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to view or configure the properties of an Outlook Web App virtual directory.

If you use the Shell to view the properties of an Outlook Web App virtual directory, the information returned is a subset of the information that's available. For example, if you use the **Get-OWAVirtualDirectory** cmdlet to view properties, Exchange returns the following information:

- Virtual directory name

- Server name

- Exchange server version

You can also retrieve information for a specific virtual directory on a specific server by using the available parameters. For more information about the parameters for the **Get-OWAVirtualDirectory** cmdlet, see [Get-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Get-OwaVirtualDirectory).

If you use the EAC to view the properties of an Outlook Web App virtual directory, you'll be able to view most of the properties for the virtual directory that you're viewing.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook Web App virtual directories" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to view or configure Outlook Web App virtual directory properties

1. In the EAC, click **Servers** \> **Virtual Directories**.

    You can use the drop-down lists to select the server and virtual directory type. By default, all servers and virtual directories are displayed.

2. In the result pane, click to select the virtual directory you want to view or edit, then click **Edit**.

3. On the **General** tab, you can view the properties of the Outlook Web App default website and specify an external URL and an internal URL. View or select the following options:

   - **Server** (read-only): **Server** displays the name of the server that hosts the Outlook Web App virtual directory.

   - **Version**: (read-only): **Version** displays the version of the Exchange server that the virtual directory is on.

   - **Web site**: (read-only.) **Web site** displays the name of the website.

   - **Outlook Web App version** (read-only): **Outlook Web App version** displays the Exchange server version.

   - **Modified** (read-only): **Modified** displays the last date and time that the virtual directory was modified.

   - **Internal URL**: In this text box, specify the URL used to access this website from an internal network. An internal URL is configured automatically during Exchange 2013 Setup. The default internal URL setting for an Internet-facing or non-Internet-facing server is `https://<Computer Name>/owa`.

   - **External URL**: In this text box, specify the URL used to access the website from the Internet. By default, **External URL** is blank. For Internet-facing Client Access servers, **External URL** should be set to the value published in DNS for that Active Directory site. For Exchange 2013 servers that don't have an Internet presence, the **External URL** setting should remain blank.

4. On the **Authentication** tab, specify the authentication methods, sign-in format, and sign-in domain.

   - **Use one or more standard authentication methods**: Select this option to use one or more of the following standard authentication methods:

     **Integrated Windows authentication**: This method requires that users have a valid Windows Server 2008 or Windows Server 2012 user account name and password to access information. Users aren't prompted for their account names and passwords. Instead, the server negotiates with the Windows security packages installed on the client computer. Integrated Windows authentication enables the server to authenticate users without prompting them for information and without transmitting information that isn't encrypted over the network. For this method to work, the client computer must be a member of the same domain as the servers running Exchange, or of a domain that's trusted by the domain that the Exchange server is in.

     **Digest authentication for Windows domain servers**: This method transmits passwords over the network as a hash value for additional security. Digest authentication can be used only in Windows Server 2008 and Windows Server 2012 domains for users who have an account that's stored in Active Directory. For more information about Digest authentication, see the Windows Server documentation.

     **Basic authentication (password is sent in clear text)**: This method is a simple authentication mechanism defined by the HTTP specification that encodes a user's sign-in name and password before the user's credentials are sent to the server. To make sure that the password is as secure as possible, you should use Secure Sockets Layer (SSL) encryption between client computers and the server that has the Client Access server role installed.

   - **Use forms-based authentication**: Forms-based authentication provides enhanced security for Outlook Web App virtual directories. Forms-based authentication creates a sign-in page for Outlook Web App. You can configure the type of sign-in prompt used by forms-based authentication. For example, you can configure forms-based authentication to require users to provide their domain and username information, in the domain\\username format on the Outlook Web App sign-in page.

     > [!IMPORTANT]
     > Forms-based authentication won't provide a secure channel unless SSL is enabled.

     Select one of the following:

     **Domain\\user name**: This requires the user to enter their domain and username in the format domain\\user name. For example, for a user named Kweku in the domain Contoso, the sign-in would be contoso\\kweku.

     **User principal name (UPN)** If the user principal name (UPN) sign-in format is specified, the **User name** box on the Outlook Web App sign-in page guides users to enter their email address, for example, kweku@contoso.com. If a user's UPN isn't identical to the email address, the user can't access Outlook Web App by using the **PrincipalName** sign-in prompt. It's a best practice to use the **PrincipalName** sign-in prompt only if users' UPNs match their email addresses.

     **Username only** The user enters their username only, without the domain name, for example, Kweku. If you use the **Username only** sign-in prompt for forms-based authentication, you must also specify the **Logon Domain** property. The **Logon Domain** property determines the default domain to use when a user tries to sign in to Outlook Web App. For example, if the default domain is Contoso, and a domain user named Kweku signs in to Outlook Web App, only Kweku must be entered as the user name. The server will use the default domain Contoso. If the user isn't a member of the Contoso domain, the domain and username must be entered.

5. On the **Features** tab, specify the features that you want to enable or disable for Outlook Web App users on a virtual directory.

   > [!NOTE]
   > Features settings for individual users override virtual directory settings. You can change segmentation settings for individual users by using the <STRONG>Set-CASMailbox</STRONG> cmdlet or by using Outlook Web App mailbox policies. For more information, see <A href="https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/outlook-on-the-web/outlook-web-app-mailbox-policies">Outlook Web App mailbox policies</A>.

   Use the check boxes to enable or disable features. By default, the most common features are displayed. To see all features that can be enabled or disabled, click **More options**.

   > [!NOTE]
   > The option to enable or disable the standard version of Outlook Web App by using the <STRONG>Premium client</STRONG> check box has been deprecated and will be removed from the settings. The standard version of Outlook Web App is always enabled.

6. On the **File access** tab, use the check boxes to configure the file access and viewing options for users. File access lets a user open or view the contents of files attached to an email message.

   File access can be controlled based on whether a user has signed in on a public or private computer. The option for users to select private computer access or public computer access are available only when you're using forms-based authentication. All other forms of authentication default to private computer access.

   - **Direct file access**: Select this check box if you want to enable direct file access. Direct file access lets users open files attached to email messages.

   - **WebReady Document Viewing**: Select this check box if you want to enable supported documents to be converted to HTML and displayed in a web browser.

   - **Force WebReady Document Viewing when a converter is available**: Select this check box if you want to force documents to be converted to HTML and displayed in a web browser before users can open them in the viewing application. Documents can be opened in the viewing application only if direct file access has been enabled.

7. Click **Save** to update the policy.

## Use the Shell to configure Outlook Web App virtual directory properties

This example enables forms-based authentication on the default Outlook Web App virtual directory on the server Contoso.

```powershell
Set-OwaVirtualDirectory -Identity "Contoso\owa (default web site)" -FormsAuthentication $true
```

For more information about syntax and parameters, see [Set-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-OwaVirtualDirectory).

## Use the Shell to view Outlook Web App virtual directory properties

This example lets you view the properties for all Outlook Web App virtual directories in all Internet Information Services (IIS) websites on all computers that have the Client Access server role installed in an Exchange organization.

```powershell
Get-OWAVirtualDirectory
```

This example lets you view the properties for an Outlook Web App virtual directory on the default IIS website on the local Exchange server.

```powershell
Get-OWAVirtualDirectory -identity "<Exchange Server Name>\owa (default web site)"
```

This example lets you view the properties for all Outlook Web App virtual directories on an IIS website on a specific Exchange server.

```powershell
Get-OWAVirtualDirectory -server <Exchange Server Name>
```

This example lets you view the values of the properties for every Outlook Web App virtual directory in all IIS websites on all Client Access servers in an Exchange organization.

```powershell
Get-OWAVirtualDirectory | format-list
```

For more information about syntax and parameters, see [Get-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Get-OwaVirtualDirectory).

## How do you know this worked?

To verify that you've successfully edited an Outlook Web App virtual directory:

1. In the EAC, click **Servers** \> **Virtual Directories**, and then choose a specific Outlook Web App virtual directory.

2. Click the **Edit** button to view the properties of the virtual directory.

3. Click **Save** or **Cancel** to close the properties page.
