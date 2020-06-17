---
title: 'Working with command output: Exchange 2013 Help'
TOCTitle: Working with command output
ms:assetid: 8320e1a5-d3f5-4615-878d-b23e2aaa6b1e
ms:mtpsurl: https://technet.microsoft.com/library/Bb123533(v=EXCHG.150)
ms:contentKeyID: 49289327
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Working with command output

_**Applies to:** Exchange Server 2013_

The Exchange Management Shell offers several methods that you can use to format command output. This topic discusses the following subjects:

- How to format data   Control how the data that you see is formatted by using the **Format-List**, **Format-Table**, and **Format-Wide** cmdlets.

- How to output data   Determine whether data is output to the Shell console window or to a file by using the **Out-Host** and **Out-File** cmdlets. Included in this topic is a sample script to output data to Microsoft Internet Explorer.

- How to filter data   Filter data by using either of the following filtering methods:

  - Server-side filtering, available on certain cmdlets.

  - Client-side filtering, available on all cmdlets by piping the results of a command to the **Where-Object** cmdlet.

To use the functionality that is described in this topic, you must be familiar with the following concepts:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Shell variables](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_automatic_variables)

- [Comparison operators](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_comparison_operators)

## How to format data

If you call formatting cmdlets at the end of the pipeline, you can override the default formatting to control what data is displayed and how that data appears. The formatting cmdlets are **Format-List**, **Format-Table**, and **Format-Wide**. Each has its own distinct output style that differs from the other formatting cmdlets.

## Format-List

The **Format-List** cmdlet takes input from the pipeline and outputs a vertical columned list of all the specified properties of each object. You can specify which properties you want to display by using the *Property* parameter. If the **Format-List** cmdlet is called without any parameters specified, all properties are output. The **Format-List** cmdlet wraps lines instead of truncating them. One of the best uses for the **Format-List** cmdlet is to override the default output of a cmdlet so that you can retrieve additional or more focused information.

For example, when you call the **Get-Mailbox** cmdlet, you only see a limited amount of information in table format. If you pipe the output of the **Get-Mailbox** cmdlet to the **Format-List** cmdlet and add parameters for the additional or more focused information that you want to view, you can retrieve the output that you want.

You can also specify a wildcard character "\*" with a partial property name. If you include a wildcard character, you can match multiple properties without having to type each property name individually. For example, `Get-Mailbox | Format-List -Property Email* `returns all properties that begin with `Email`.

The following examples show the different ways that you can view the same data returned by the **Get-Mailbox** cmdlet.

```powershell
Get-Mailbox TestUser1

Name                      Alias                ServerName       ProhibitSendQuota
----                      -----                ----------       ---------------
TestUser1                 TestUser1            mbx              unlimited
```

In the first example, the **Get-Mailbox** cmdlet is called without specific formatting so the default output is in table format and contains a predetermined set of properties.

```powershell
Get-Mailbox TestUser1 | Format-List -Property Name,Alias,EmailAddresses

Name           : TestUser1
Alias          : TestUser1
EmailAddresses : {SMTP:TestUser1@contoso.com}
```

In the second example, the output of the **Get-Mailbox** cmdlet is piped to the **Format-List** cmdlet, together with specific properties. As you can see, the format and content of the output is significantly different.

```powershell
Get-Mailbox TestUser1 | Format-List -Property Name, Alias, Email*
Name                      : Test User
Alias                     : TestUser1
EmailAddresses            : {SMTP:TestUser1@contoso.com}
EmailAddressPolicyEnabled : True
```

In the last example, the output of the **Get-Mailbox** cmdlet is piped to the **Format-List** cmdlet as in the second example. However, in the last example, a wildcard character is used to match all properties that start with `Email`.

If more than one object is passed to the **Format-List** cmdlet, all specified properties for an object are displayed and grouped by object. The display order depends on the default parameter for the cmdlet. The default parameter is most frequently the *Name* parameter or the *Identity* parameter. For example, when the **Get-Childitem** cmdlet is called, the default display order is file names in alphabetical order. To change this behavior, you must call the **Format-List** cmdlet, together with the *GroupBy* parameter, and the name of a property value by which you want to group the output. For example, the following command lists all files in a directory and then groups these files by extension.

```powershell
Get-Childitem | Format-List Name,Length -GroupBy Extension

Extension: .xml

Name   : Config_01.xml
Length : 5627

Name   : Config_02.xml
Length : 3901

Extension: .bmp

Name   : Image_01.bmp
Length : 746550

Name   : Image_02.bmp
Length : 746550

Extension: .txt

Name   : Text_01.txt
Length : 16822

Name   : Text_02.txt
Length : 9835
```

In this example, the **Format-List** cmdlet has grouped the items by the *Extension* property that is specified by the *GroupBy* parameter. You can use the *GroupBy* parameter with any valid property for the objects in the pipeline stream.

## Format-Table

The **Format-Table** cmdlet lets you display items in a table format with label headers and columns of property data. By default, many cmdlets, such as the **Get-Process** and **Get-Service** cmdlets, use the table format for output. Parameters for the **Format-Table** cmdlet include the *Properties* and *GroupBy* parameters. These parameters work exactly as they do with the **Format-List** cmdlet.

The **Format-Table** cmdlet also uses the *Wrap* parameter. This parameter enables long lines of property information to display completely instead of truncating at the end of a line. To see how the *Wrap* parameter is used to display returned information, compare the output of the **Get-Command** command in the following two examples.

In the first example, when the **Get-Command** cmdlet is used to display command information about the **Get-Process** cmdlet, the information for the *Definition* property is truncated.

```powershell
Get-Command Get-Process | Format-Table Name,Definition

Name                                    Definition
----                                    ----------
get-process                             get-process [[-ProcessName] String[]...
```

In the second example, the *Wrap* parameter is added to the command to force the complete contents of the *Definition* property to display.

```powershell
Get-Command Get-Process | Format-Table Name,Definition -Wrap

Get-Process                             Get-Process [[-Name] <String[]>] [-Comp
                                        uterName <String[]>] [-Module] [-FileVe
                                        rsionInfo] [-Verbose] [-Debug] [-ErrorA
                                        ction <ActionPreference>] [-WarningActi
                                        on <ActionPreference>] [-ErrorVariable
                                        <String>] [-WarningVariable <String>] [
                                        -OutVariable <String>] [-OutBuffer <Int
                                            32>]
                                        Get-Process -Id <Int32[]> [-ComputerNam
                                        e <String[]>] [-Module] [-FileVersionIn
                                        fo] [-Verbose] [-Debug] [-ErrorAction <
                                        ActionPreference>] [-WarningAction <Act
                                        ionPreference>] [-ErrorVariable <String
                                        >] [-WarningVariable <String>] [-OutVar
                                        iable <String>] [-OutBuffer <Int32>]
                                        Get-Process [-ComputerName <String[]>]
                                        [-Module] [-FileVersionInfo] -InputObje
                                        ct <Process[]> [-Verbose] [-Debug] [-Er
                                        rorAction <ActionPreference>] [-Warning
                                        Action <ActionPreference>] [-ErrorVaria
                                        ble <String>] [-WarningVariable <String
                                        >] [-OutVariable <String>] [-OutBuffer
                                        <Int32>]
```

As with the **Format-List** cmdlet, you can also specify a wildcard character "`*`" with a partial property name. By including a wildcard character, you can match multiple properties without typing each property name individually.

## Format-Wide

The **Format-Wide** cmdlet provides a much simpler output control than the other format cmdlets. By default, the **Format-Wide** cmdlet tries to display as many columns of property values as possible on a line of output. By adding parameters, you can control the number of columns and how the output space is used.

In the most basic usage, calling the **Format-Wide** cmdlet without any parameters arranges the output in as many columns as will fit the page. For example, if you run the **Get-Childitem** cmdlet and pipe its output to the **Format-Wide** cmdlet, you will see the following display of information:

```powershell
Get-ChildItem | Format-Wide

Directory: FileSystem::C:\WorkingFolder

Config_01.xml                           Config_02.xml
Config_03.xml                           Config_04.xml
Config_05.xml                           Config_06.xml
Config_07.xml                           Config_08.xml
Config_09.xml                           Image_01.bmp
Image_02.bmp                            Image_03.bmp
Image_04.bmp                            Image_05.bmp
Image_06.bmp                            Text_01.txt
Text_02.txt                             Text_03.txt
Text_04.txt                             Text_05.txt
Text_06.txt                             Text_07.txt
Text_08.txt                             Text_09.txt
Text_10.txt                             Text_11.txt
Text_12.txt
```

Generally, calling the **Get-Childitem** cmdlet without any parameters displays the names of all files in the directory in a table of properties. In this example, by piping the output of the **Get-Childitem** cmdlet to the **Format-Wide** cmdlet, the output was displayed in two columns of names. Notice that only one property type can be displayed at a time, specified by a property name that follows the **Format-Wide** cmdlet. If you add the *Autosize* parameter, the output is changed from two columns to as many columns as can fit the screen width.

```powershell
Get-ChildItem | Format-Wide -AutoSize

Directory: FileSystem::C:\WorkingFolder

Config_01.xml   Config_02.xml   Config_03.xml   Config_04.xml   Config_05.xml
Config_06.xml   Config_07.xml   Config_08.xml   Config_09.xml   Image_01.bmp
Image_02.bmp    Image_03.bmp    Image_04.bmp    Image_05.bmp    Image_06.bmp
Text_01.txt     Text_02.txt     Text_03.txt     Text_04.txt     Text_05.txt
Text_06.txt     Text_07.txt     Text_08.txt     Text_09.txt     Text_10.txt
Text_11.txt     Text_12.txt
```

In this example, the table is arranged in five columns, instead of two columns. The *Column* parameter offers more control by letting you specify the maximum number of columns to display information as follows:

```powershell
Get-ChildItem | Format-Wide -Column 4

Directory: FileSystem::C:\WorkingFolder

Config_01.xml       Config_02.xml       Config_03.xml       Config_04.xml
Config_05.xml       Config_06.xml       Config_07.xml       Config_08.xml
Config_09.xml       Image_01.bmp        Image_02.bmp        Image_03.bmp
Image_04.bmp        Image_05.bmp        Image_06.bmp        Text_01.txt
Text_02.txt         Text_03.txt         Text_04.txt         Text_05.txt
Text_06.txt         Text_07.txt         Text_08.txt         Text_09.txt
Text_10.txt         Text_11.txt         Text_12.txt
```

In this example, the number of columns is forced to four by using the *Column* parameter.

## How to output data

## Out-Host and Out-File cmdlets

The **Out-Host** cmdlet is an unseen default cmdlet at the end of the pipeline. After all formatting is applied, the **Out-Host** cmdlet sends the final output to the console window for display. You don't have to explicitly call the **Out-Host** cmdlet, because it's the default output. You can override sending the output to the console window by calling the **Out-File** cmdlet as the last cmdlet in the command. The **Out-File** cmdlet then writes the output to the file that you specify in the command as in the following example:

```powershell
Get-ChildItem | Format-Wide -Column 4 | Out-File c:\OutputFile.txt
```

In this example, the **Out-File** cmdlet writes the information that is displayed in the **Get-ChildItem | Format-Wide -Column 4** command to a file that is named `OutputFile.txt`. You can also redirect pipeline output to a file by using the redirection operator, which is the right-angle bracket ( `>` ). To append pipeline output of a command to an existing file without replacing the original file, use the double right-angle brackets ( `>>` ), as in the following example:

```powershell
Get-ChildItem | Format-Wide -Column 4 >> C:\OutputFile.txt
```

In this example, the output from the **Get-Childitem** cmdlet is piped to the **Format-Wide** cmdlet for formatting and then is written to the end of the `OutputFile.txt` file. Notice that if the `OutputFile.txt` file didn't exist, use of the double right-angle brackets ( `>>` ) would create the file.

For more information about pipelines, see [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines).

For more information about the syntax used in the previous examples, see [Syntax](https://docs.microsoft.com/powershell/exchange/exchange-cmdlet-syntax).

## Viewing data in Internet Explorer

Because of the flexibility and ease of scripting in the Exchange Management Shell, you can take the data that is returned by commands and format and output them in almost limitless ways.

The following example shows how you can use a simple script to output the data that is returned by a command and display it in Internet Explorer. This script takes the objects that are passed through the pipeline, opens an Internet Explorer window, and then displays the data in Internet Explorer:

```powershell
$Ie = New-Object -Com InternetExplorer.Application
$Ie.Navigate("about:blank")
While ($Ie.Busy) { Sleep 1 }
$Ie.Visible = $True
$Ie.Document.Write("$Input")
# If the previous line doesn't work on your system, uncomment the line below.
# $Ie.Document.IHtmlDocument2_Write("$Input")
$Ie
```

To use this script, save it to the `C:\Program Files\Microsoft\Exchange Server\V15\Scripts` directory on the computer where the script will be run. Name the file `Out-Ie.ps1`. After you save the file, you can then use the script as a regular cmdlet.

> [!NOTE]
> To run scripts in Exchange 2013, scripts must be added to an unscoped management role and you must be assigned the management role either directly or through a management role group. For more information, see <A href="understanding-management-roles-exchange-2013-help.md">Understanding management roles</A>.

The `Out-Ie` script assumes that the data it receives is valid HTML. To convert the data that you want to view into HTML, you must pipe the results of your command to the **ConvertTo-Html** cmdlet. You can then pipe the results of that command to the `Out-Ie` script. The following example shows how to view a directory listing in an Internet Explorer window:

```powershell
Get-ChildItem | Select Name,Length | ConvertTo-Html | Out-Ie
```

## How to filter data

The Shell gives you access to a large quantity of information about your servers, mailboxes, Active Directory, and other objects in your organization. Although access to this information helps you better understand your environment, this much information can be overwhelming. The Shell lets you control this information and return only the data that you want to see by using filtering. The following types of filtering are available:

- **Server-side filtering**: Server-side filtering takes the filter that you specify on the command line and submits it to the Exchange server that you query. That server processes the query and returns only the data that matches the filter that you specified.

    Server-side filtering is performed only on objects where tens or hundreds of thousands of results could be returned. Therefore, only the recipient management cmdlets, such as the **Get-Mailbox** cmdlet, and queue management cmdlets, such as the **Get-Queue** cmdlet, support server-side filtering. These cmdlets support the *Filter* parameter. This parameter takes the filter expression that you specify and submits it to the server for processing.

- **Client-side filtering**: Client-side filtering is performed on the objects in the local console window in which you are currently working. When you use client-side filtering, the cmdlet retrieves all the objects that match the task that you are performing to the local console window. The Shell then takes all the returned results, applies the client-side filter to those results, and returns to you only the results that match your filter. All cmdlets support client-side filtering. This is invoked by piping the results of a command to the **Where-Object** cmdlet.

## Server-side filtering

The implementation of server-side filtering is specific to the cmdlet on which it is supported. Server-side filtering is enabled only on specific properties on the objects that are returned. For more information, see the Help for the following cmdlets:

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Get-ActiveSyncDevice</p></td>
<td><p>Get-ActiveSyncDeviceClass</p></td>
<td><p>Get-CASMailbox</p></td>
<td><p>Get-Contact</p></td>
<td><p>Get-DistributionGroup</p></td>
</tr>
<tr class="even">
<td><p>Get-DynamicDistributionGroup</p></td>
<td><p>Get-Group</p></td>
<td><p>Get-Mailbox</p></td>
<td><p>Get-MailboxStatistics</p></td>
<td><p>Get-MailContact</p></td>
</tr>
<tr class="odd">
<td><p>Get-MailPublicFolder</p></td>
<td><p>Get-MailUser</p></td>
<td><p>Get-Message</p></td>
<td><p>Get-MobileDevice</p></td>
<td><p>Get-Queue</p></td>
</tr>
<tr class="even">
<td><p>Get-QueueDigest</p></td>
<td><p>Get-Recipient</p></td>
<td><p>Get-RemoteMailbox</p></td>
<td><p>Get-RoleGroup</p></td>
<td><p>Get-SecurityPrincipal</p></td>
</tr>
<tr class="odd">
<td><p>Get-StoreUsageStatistics</p></td>
<td><p>Get-UMMailbox</p></td>
<td><p>Get-User</p></td>
<td><p>Get-UserPhoto</p></td>
<td><p>Remove-Message</p></td>
</tr>
<tr class="even">
<td><p>Resume-Message</p></td>
<td><p>Resume-Queue</p></td>
<td><p>Retry-Queue</p></td>
<td><p>Suspend-Message</p></td>
<td><p>Suspend-Queue</p></td>
</tr>
</tbody>
</table>

## Client-side filtering

Client-side filtering can be used with any cmdlet. This capability includes those cmdlets that also support server-side filtering. As described earlier in this topic, client-side filtering accepts all the data that is returned by a previous command in the pipeline, and in turn, returns only the results that match the filter that you specify. The **Where-Object** cmdlet performs this filtering. It can be shortened to **Where**.

As data passes through the pipeline, the **Where** cmdlet receives the data from the previous object and then filters the data before passing it on to the next object. The filtering is based on a script block that is defined in the **Where** command. The script block filters data based on the object's properties and values.

The **Clear-Host** cmdlet is used to clear the console window. In this example, you can find all the defined aliases for the **Clear-Host** cmdlet if you run the following command:

```powershell
Get-Alias | Where {$_.Definition -eq "Clear-Host"}

CommandType     Name                            Definition
-----------     ----                            ----------
Alias           clear                           clear-host
Alias           cls                             clear-host
```

The **Get-Alias** cmdlet and the **Where** command work together to return the list of aliases that are defined for the **Clear-Host** cmdlet and no other cmdlets. The following table outlines each element of the **Where** command that is used in the example.

### Elements of the Where command

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Element</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>{ }</code></p></td>
<td><p>Braces enclose the script block that defines the filter.</p></td>
</tr>
<tr class="even">
<td><p><code>$_</code></p></td>
<td><p>This special variable automatically initiates and binds to the objects in the pipeline.</p></td>
</tr>
<tr class="odd">
<td><p><code>Definition</code></p></td>
<td><p>The <code>Definition</code> property is the property of the current pipeline objects that stores the name of the alias definition. When <code>Definition</code> is used with the <code>$_</code> variable, a period comes before the property name.</p></td>
</tr>
<tr class="even">
<td><p><code>-eq</code></p></td>
<td><p>This comparison operator for "equal to" is used to specify that the results must exactly match the property value that is supplied in the expression.</p></td>
</tr>
<tr class="odd">
<td><p>&quot;<strong>Clear-Host</strong>&quot;</p></td>
<td><p>In this example, &quot;<strong>Clear-Host</strong>&quot; is the value for which the command is parsing.</p></td>
</tr>
</tbody>
</table>

In the example, the objects that are returned by the **Get-Alias** cmdlet represent all the defined aliases on the system. Even though you don't see them from the command line, the aliases are collected and passed to the **Where** cmdlet through the pipeline. The **Where** cmdlet uses the information in the script block to apply a filter to the alias objects.

The special variable `$`\_represents the objects that are being passed. The `$_`variable is automatically initiated by the Shell and is bound to the current pipeline object. For more information about this special variable, see [Shell variables](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_automatic_variables).

Using standard "dot" notation (object.property), the `Definition` property is added to define the exact property of the object to evaluate. The `-eq` comparison operator then compares the value of this property to `"Clear-Host"`. Only the objects that have the `Definition` property that match this criterion are passed to the console window for output. For more information about comparison operators, see [Comparison operators](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_comparison_operators).

After the **Where** command has filtered the objects returned by the **Get-Alias** cmdlet, you can pipe the filtered objects to another command. The next command processes only the filtered objects returned by the Where command.
