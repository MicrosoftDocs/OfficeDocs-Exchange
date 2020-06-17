---
title: 'Customize the Outlook Web App sign-In, language selection, and error pages'
TOCTitle: Customize the Outlook Web App sign-In, language selection, and error pages
ms:assetid: d8d9f735-7181-428f-9049-b9886dce5159
ms:mtpsurl: https://technet.microsoft.com/library/Ee633483(v=EXCHG.150)
ms:contentKeyID: 53957627
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Customize the Outlook Web App sign-In, language selection, and error pages

_**Applies to:** Exchange Server 2013_

This topic explains how to customize the color and images of the sign-in, language selection, and error pages for Outlook Web App.

The Outlook Web App sign-in, language selection, and error pages are created based on graphics and .css files in the themes resources folder. Outlook Web App uses only one set of sign-in, language selection, and error pages for all themes. Any modifications to those pages will be seen by all users. You can find the theme resources folder in the Exchange installation directory at V15\\FrontEnd\\HttpProxy\\owa\\auth\\version\\themes\\resources. You'll need a text editor to change the default colors, and a graphics editor to change the images. If you must match a specific color and you can't find a match for it at [Color Table](https://go.microsoft.com/fwlink/p/?linkid=280679), you can use an image editing tool to sample a color and determine its HTML RGB value.

If you have multiple servers supporting Outlook Web App and want them all to use the same sign-in, language, and error pages, you must copy the modified files to each server. You should also create a back-up copy of your customized files. If you reinstall or upgrade Exchange, all files in the themes folders will be overwritten. You'll have to copy your customized files back to the appropriate folder after the reinstallation or upgrade is complete.

> [!IMPORTANT]
> Back up copies of all the files that you'll be changing before you start to create your custom sign-in and sign-out pages.

For information about creating a custom theme, see [Create a theme for Outlook Web App](create-a-theme-for-outlook-web-app-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 45 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Graphics editor" entry under "Outlook Web App Permissions" in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Customize the color of the sign-in page

1. Log on to the Exchange server and use Windows Explorer to go to the Exchange server installation directory and find \\V15\\FrontEnd\\HttpProxy\\owa\\auth\\\<version\>\\themes\\resources.

2. Use a text editor, such as Notepad, to open logon.css.

3. Search for the default color value \#0072c6 and replace it with the HTML RGB value for the color you want to use.

4. Save and close the file.

## Customize the color of the error page

1. Log on to the Exchange server and use Windows Explorer to go to the Exchange server installation directory and find \\V15\\FrontEnd\\HttpProxy\\owa\\auth\\\<version\>\\themes\\resources.

2. Use a text editor, such as Notepad, to open errorFE.css.

3. Search for the default color value \#0072c6 and replace it with the HTML RGB value for the color you want to use.

4. Save and close the file.

## Customize the color of the language selection page

1. Log on to the Exchange server, then use Windows Explorer to go to the Exchange server installation directory and find \\V15\\ClientAccess\\Owa\\prem\\\<version\>\\resources\\styles.

2. Use a text editor, such as Notepad, to open LanguageSelection.css.

3. Search for the default color value \#0072c6 and replace it with the HTML RGB value for the color you want to use.

4. Save and close the file.

## Customize the images on the sign-in and error pages

Use an image editing tool to open and edit the images used to build the sign-in and error pages.

1. Log on to the Exchange server and use Windows Explorer to go to the Exchange server installation directory and find \\V15\\FrontEnd\\HttpProxy\\owa\\auth\\\<version\>\\themes\\resources.

2. Use a graphics editor to open and modify the following files:

   - owa\_text\_blue.png, to change the "Outlook Web App" text logo.

   - olk\_logo\_white.png, to change the app logo in the left bar.

   - olk\_logo\_white\_cropped.png, to change the image in the left side panel of the error page.

   - sign\_in\_arrow.png, to change the icon left of the "sign in" button.

   - olk\_exchange\_text\_blue.png, to change the "Outlook Mobile" logo on tnarrow layout.

   - olk\_logo\_white\_small.png is used in tnarrow.

   - olk\_exchange\_text\_stacked\_white\_small.png is used in tnarrow.

3. Search for the default color value \#0072c6 and replace it with the HTML RGB value for the color you want to use.

4. Save and close the file.

## How do you know this worked?

Open the Outlook Web App sign-in page in Internet Explorer. If you're testing the changes to the default website on the server that's hosting the Outlook Web App virtual directory, you can test them by opening Internet Explorer and entering the URL https://localhost/owa.

If you don't see your changes, do the following:

1. On the toolbar, select **Settings** \> **Internet options** \> **General**.

2. Under **Browsing history**, select **Delete**.

3. Select **Temporary Internet files and website files**, and then select **Delete**.

4. When Internet Explorer is finished deleting, select **OK** to close **Internet options**.

5. Refresh the browser window.

> [!TIP]
> To see the effects of your changes, you can keep the .css file that you're editing open and refresh the browser window after saving each change.
