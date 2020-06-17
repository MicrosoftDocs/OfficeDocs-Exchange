---
title: 'Create a theme for Outlook Web App: Exchange 2013 Help'
TOCTitle: Create a theme for Outlook Web App
ms:assetid: 7e1fa13c-3de3-45c2-b1fa-e74fc8487bda
ms:mtpsurl: https://technet.microsoft.com/library/Bb201700(v=EXCHG.150)
ms:contentKeyID: 53957626
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a theme for Outlook Web App

_**Applies to:** Exchange Server 2013_

A *theme* defines the background color, fonts, highlight colors, icons, and header that are used by Microsoft Outlook Web App. Each theme is a collection of media files and cascading style sheets (.css) files that are stored on the Microsoft Exchange server in the installation directory in \\Client Access\\OWA\\prem\\*version*\\resources\\themes. Each theme is stored in its own subdirectory of \\themes.

The default theme is found in \\Client Access\\OWA\\prem\\*version*\\resources\\themes\\base. Each theme folder contains all the files that are needed to define a theme. These files include CSS files, graphics, and an .xml file that defines the name of the theme. Additional themes are created by copying all the files from one theme into a new folder and modifying those files as needed.

By default, multiple themes are installed when you install Exchange Server 2013, as follows:

- CSS (.css) files define colors, gradients, and fonts.

- Image (.png) files provide the icons and other graphic elements. If you edit any of the icons, don't change their size. If you change the size of any graphic elements, test your changes to verify that the elements still fit together correctly.

These files are stored on the Client Access server in the installation directory in \\Client Access\\OWA\\prem\\*\<version\>*\\resources\\themes. Each theme is stored in a subdirectory of themes. You can create additional themes by copying an existing theme and modifying the copy.

After you create a theme, you may also want to [Customize the Outlook Web App sign-In, language selection, and error pages](customize-the-outlook-web-app-sign-in-language-selection-and-error-pages-exchange-2013-help.md).

> [!NOTE]
> The light version of Outlook Web App doesn't support themes.

> [!WARNING]
> If you have multiple servers that support Outlook Web App, you must copy your custom theme to each server. You should also create a backup copy of your custom theme. If you reinstall or upgrade Exchange, all files in the themes folders will be overwritten. You'll have to copy your theme back to the appropriate folder after the reinstallation or upgrade is complete.<BR>Make backup copies of all the files that you'll be changing before you start to create your custom theme.

## What do you need to know before you begin?

- Estimated time to complete this task: 60 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook Web App virtual directories" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- You need local server administrator access to perform these procedures.

- You'll need a text editor to change the default colors, and a graphics editor to change the images. If you must match a specific color and you can't find a match for it at [Color Table](https://go.microsoft.com/fwlink/p/?linkid=280679), you can use an image editing tool to sample a color and determine its HTML RGB value.

- As a best practice, we recommend that you use the following guidelines any time that you change or create an Outlook Web App theme:

  - If you decide to edit an existing theme, make backup copies of the original files before you start editing them.

  - Do not delete the folder \\Client Access\\OWA\\prem\\*version*\\resources\\themes\\base or any of the files in it.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Create a new Outlook Web App theme

To start, you'll create a folder for a new theme, and then copy the files from an existing theme into the new folder.

1. Log on to the Exchange server that is hosting the Outlook Web App virtual directory by using an account that has been delegated membership in the local Administrators group.

2. Open Windows Explorer, and then find the Exchange server installation directory.

3. In \\Client Access\\OWA\\prem\\*version*\\resources\\themes, create a new folder and name it, for example, Fourth Coffee.

4. Copy all the files from another theme to the new folder.

## Step 2: Name your new theme

To set the display name for your new theme, do the following:

1. Open the copy of themeinfo.xml that's in the custom theme folder you just created.

2. Find the theme `displayname` value, and change the value to the name you want to use. For example `displayname = "Fourth Coffee Theme"`.

3. Save and close themeinfo.xml.

## Step 3: Change the sort order of your new theme (optional)

If you want, you can change the sort order of the new theme by editing the themeinfo.xml file. The sort order determines the theme position in the **Change theme** panel in the Settings menu.

To change the sort order of the new theme by using the themeinfo.xml file, do the following:

1. Open the copy of themeinfo.xml that's in the custom theme folder.

2. Find the theme `sortorder` value, and change the value to reflect where you want your new theme to appear in the list. The themes will be ordered by the numeric value in increasing order. By default, the base theme is the first one and its `sortorder` value is "0". For example `sortorder="<number>"`.

3. Save and close themeinfo.xml.

## Step 4: Modify your new theme

Now that you've copied over the files and have named your theme, you can customize it. The following elements can be customized in an Outlook Web App theme:

- Image files, which define the header area and icons.

- CSS files, which define fonts and colors.

### Image files

Theme images are stored in two folders in \\themes*\\\<theme name\>*\\images\\. The \\images\\0 folder contains images that will be used in left-to-right languages (like English), and languages that are read from right to left will use the images in the \\images\\rtl folder.

> [!NOTE]
> Some of the images in the \images\rtl folder are the same the images as in the \images\0 folder, but they're mirrored.

To customize the theme, you can use an image editing tool to open and modify the following images:

- Headerbgmain.png

  This is the main header image. We recommend that the image doesn't exceed the header height of 30 pixels. The default theme doesn't use a background image, so this image is transparent. For an example of a theme that has a custom background image, see the image in the **Blueprint** theme folder.

- Headerbgright.png

  This is used as a tiling image behind the header. The default theme doesn't use a tiling background image, so this image is transparent. For an example of a theme that has a custom tiling background image, see the image in the **Blueprint** theme folder.

- sprite1.mouse.png

  This contains most of the images used in a theme. You can change the color of the images to match your theme, and also change the default Outlook Web App text logo.

  To avoid any issues, don't change the size of any individual icons in the sprite, and make sure that it's saved as a transparent .png file.

- themepreview.png

  This image will be used to represent the theme in the **Change theme** panel in the Settings menu in Outlook Web App.

### Colors and fonts

Cascading style sheet (.css) files define the colors and fonts used in a theme and are stored in multiple folders under \\themes\\*\<theme name\>*. The \\*\<theme name\>*\\0 folder contains .css files that will be used in left-to-right languages (like English), and languages that are read from right to left will use the .css files in the \\*\<theme name\>*\\rtl folder. There are also language-specific folders, (for example, \\ja, \\ko, \\zhs, and \\zht) that contain .css files to be used with those languages.

Start by modifying the \\*\<theme name\>*\\images\\0 folder. There are four colors used throughout each theme that can be customized.

- BrandColor: \#0072C6

- NavBarHoverColor: \#4C9CD7

- UnreadColor: \#2A8DD4

- FocusColor: \#DFEDFA

You can use a text editor like Notepad to search for and replace all the instances of these values with the colors of your theme in the following two files: owa2styles.mouseCSS and owa2styles2.mouseCSS. This has to be done in every folder in your new theme that contains those .css files.

## Step 5: Set the default Outlook Web App theme

Setting a new default theme will only affect users who haven't changed their theme through the Settings menu in Outlook Web App.

To force all users to use the default theme, you must disable theme selection in addition to setting a default theme.

### Use the Shell to set the default theme for Outlook Web App

This example sets the default theme for Outlook Web App, where the server name is `fourthcoffee`, the virtual directory name is `owa`, the website name is `default web site`, and the theme is in the folder named `Custom`.

```powershell
set-owavirtualdirectory -identity "fourthcoffee\owa (default web site)" -defaulttheme Custom
```

For detailed syntax and parameter information, see [Set-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-OwaVirtualDirectory).

### Use the Shell to disable theme selection for Outlook Web App

This example disables theme selection in Outlook Web App, where the server name is `fourthcoffee`, the virtual directory name is `owa`, and the website name is `default web site`.

```powershell
set-owavirtualdirectory -identity "fourthcoffee\owa (default web site)" -themeselectionenabled $false
```

You can also complete both commands at the same time as shown in the following example:

```powershell
set-owavirtualdirectory -identity "fourthcoffee\owa (default web site)" -defaulttheme Custom -themeselectionenabled $false
```

For detailed syntax and parameter information, see [Set-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-OwaVirtualDirectory).

## Step 6: Run iisreset/noforce to save your changes

If you add or change a theme, change the name of a theme, or change the sort order of a theme, you must stop and start Internet Information Services (IIS) for the change to take effect. To do this, open a Command Prompt window on the server where you've created your new theme, and run the command **iisresest /nforce**.

## How do you know this task worked?

1. Sign in to Outlook Web App using the virtual directory on the server where you've created your new theme. If you're testing the changes to the default website on the Exchange server that's hosting the Outlook Web App virtual directory, you can test your theme by opening Internet Explorer and entering the URL https://localhost/owa.

2. Switch to your custom theme by selecting the Settings menu \> **Change theme** and selecting your custom theme.

## If you don't see your latest changes and have run iisreset/noforce

1. On the Internet Explorer toolbar, select the Settings menu \> **Internet options**.

2. On the **General** tab, under **Browsing history**, select **Delete**, and then verify that **Temporary Internet files and website files** is checked. Then select **Delete** to remove those files.

3. Select **OK** to close **Internet options**.

4. Select **Refresh** to see your changes.

You may have to repeat these steps to see your changes every time that you make a change to the theme files. If you're making several changes, you can leave Outlook Web App open and repeat running **iisreset/noforce** on the server and clearing temporary files from Internet Explorer as needed.
