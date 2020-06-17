---
title: 'Import and export files in the Exchange Management Shell: Exchange 2013 Help'
TOCTitle: Import and export files in the Exchange Management Shell
ms:assetid: b4b669e8-a3aa-4b0b-ad34-f1f15d9c9369
ms:mtpsurl: https://technet.microsoft.com/library/Dd638170(v=EXCHG.150)
ms:contentKeyID: 50117647
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Import and export files in the Exchange Management Shell

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 uses Windows PowerShell command-line interface remoting to establish a connection between the server or workstation from which you're administering Exchange and the server running Exchange 2013 that you're administering. In Exchange 2013, this is called remote Exchange Management Shell, or remote Shell. Even if you're administering the local Exchange 2013 server, remote Shell is used to make the connection. For more information about local and remote Shell, see [Using PowerShell with Exchange 2013 (Exchange Management Shell)](https://docs.microsoft.com/powershell/exchange/exchange-management-shell).

How you import and export files to and from an Exchange server in Exchange 2013 is different than how you might have done it in Exchange Server 2007. This is due to the use of remote Shell in Exchange 2013. This topic discusses why this new process is required and how to import and export files between a local server or workstation and an Exchange 2013 server.

## Windows PowerShell sessions

To understand why you need a special syntax to import and export files in remote Shell, you need to know how the Shell is implemented in Exchange 2013. The Shell uses Windows PowerShell sessions, which are the environments in which variables, cmdlets, and so on, can share information. Every time you open a new Shell window, you create a new session. The cmdlets that are run in each window can access variables and other information stored in that window, but can't access variables in other open Shell windows. This is because they're each contained within their own Windows PowerShell session. Windows PowerShell sessions can also be referred to as runspaces.

Remote Shell in Exchange 2013 has two sessions, the local session and the remote session. The local session is the Windows PowerShell session that's running on your local computer. This session contains all of the cmdlets that ship with Windows PowerShell. It also has access to your local file system.

The remote session is the Windows PowerShell session that's running on the remote Exchange server. This session is where all Exchange cmdlets are run. It has access to the Exchange server's file system.

When you connect to a remote Exchange server, a connection is made between your local session on your computer and the remote session on the Exchange server. This connection enables you to run Exchange cmdlets on the remote Exchange server in your local session even though your local computer doesn't have any Exchange cmdlets installed. Even though the Exchange cmdlets appear to be running on your local computer, they're actually running on the Exchange server.

> [!IMPORTANT]
> Even if you open the Shell on an Exchange 2013 server, the same connection process takes place and two sessions are created. This means that you must use the same new syntax to import and export files whether you're opening the Shell on an Exchange 2013 server or from a remote client workstation.

The Exchange cmdlets that run in the remote session on the remote Exchange server don't have access to your local file system. This means that you can't use Exchange cmdlets, on their own, to import or export files from or to your local file system. Additional syntax needs to be used to transfer the files to and from your local file system so that the Exchange cmdlets running on the remote Exchange server can use the data. For more information about the required syntax, see "Importing and exporting files in remote Shell" later in this topic.

## Importing and exporting files in remote Shell

Importing and exporting files requires a specific syntax because Mailbox and Client Access servers use remote Shell and don't have access to the local computer's file system.

## Importing files in remote Shell

The syntax to import files in Exchange 2013 is used any time you want to send a file to a cmdlet running on an Exchange 2013 server from your local computer or server. Cmdlets that accept data from a file on your local computer will have a parameter called *FileData* (or something similar). To determine the correct parameter to use, see the Help information for the cmdlet you're using.

The Shell must know what file you want to send to the Exchange 2013 cmdlet, and what parameter will accept the data. To do so, use the following syntax.

```powershell
<Cmdlet> -FileData ([Byte[]]$(Get-Content -Path <local path to file> -Encoding Byte -ReadCount 0))
```

For example, the following command imports the file C:\\MyData.dat into the *FileData* parameter on the **Import-SomeData** fictional cmdlet.

```powershell
Import-SomeData -FileData (Byte[]]$(Get-Content -Path "C:\MyData.dat" -Encoding Byte -ReadCount 0))
```

The following actions occur when the command is run:

1. The command is accepted by remote Shell.

2. Remote Shell evaluates the command and determines that there's an embedded command in the value being provided to the *FileData* parameter.

3. Remote Shell stops evaluating the **Import-SomeData** command and runs the **Get-Content** command. The **Get-Content** command reads the data from the MyData.dat file.

4. Remote Shell temporarily stores the data from the **Get-Content** command as a `Byte[]` object so that it can be passed to the **Import-SomeData** cmdlet.

5. Execution of the **Import-SomeData** command resumes. Remote Shell sends the request to run the **Import-SomeData** cmdlet to the remote Exchange 2013 server, along with the object created by the **Get-Content** cmdlet.

6. On the remote Exchange 2013 server, the **Import-SomeData** cmdlet is run, and the data stored in the temporary object created by the **Get-Content** cmdlet is passed to the *FileData* parameter. The **Import-SomeData** cmdlet processes the input and performs whatever actions are required.

Some cmdlets use the following alternate syntax that accomplishes the same thing as the preceding syntax.

```powershell
[Byte[]]$Data = Get-Content -Path <local path to file> -Encoding Byte -ReadCount 0
Import-SomeData -FileData $Data
```

The same process happens with this alternate syntax. The only difference is instead of performing the entire operation at once, the data retrieved from the local file is stored in a variable that can be referenced after it's created. The variable is then used in the import command to pass the contents of the local file to the **Import-SomeData** cmdlet. Using this two-step process is useful when you want to use the data from the local file in more than one command.

There are limitations that you must consider when importing files. For more information, see "Limitations on importing files" later in this topic.

For specific information about how to import data into Exchange 2013, see the Help topics for the feature you're managing.

## Limitations on importing files

Limits must be set when importing data in remote Shell to preserve the integrity of the data that's being transferred. Transfers that are in progress can't be resumed if they're interrupted. Also, because data being transferred is stored in the remote server's memory, the server must be protected from memory exhaustion caused by excessively large amounts of data.

For these reasons, the amount of data that's transferred to a remote Exchange 2013 server from a local computer or server is limited to the following:

- 500 megabytes (MB) for each cmdlet that's run

- 75 MB for each object that's passed to a cmdlet

If you exceed either of the limits, the execution of the cmdlet and its associated pipeline will stop and you'll receive an error. Consider the examples in the following table to understand how these limits work.

### Import data limit examples

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Number of objects</th>
<th>Object size (MB)</th>
<th>Total size (MB)</th>
<th>Result of operation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>10</p></td>
<td><p>40</p></td>
<td><p>400</p></td>
<td><p>The operation is successful because neither the size of the individual objects exceeds 75 MB nor the total amount of data passed to the cmdlet exceeds 500 MB.</p></td>
</tr>
<tr class="even">
<td><p>5</p></td>
<td><p>80</p></td>
<td><p>400</p></td>
<td><p>The operation fails because, although the total amount of data passed to the cmdlet is only 400 MB, the size of each individual object exceeds the 75 MB limit.</p></td>
</tr>
<tr class="odd">
<td><p>120</p></td>
<td><p>5</p></td>
<td><p>600</p></td>
<td><p>The operation fails because, although each individual object is only 5 MB, the total amount of data passed to the cmdlet exceeds the 500 MB limit.</p></td>
</tr>
</tbody>
</table>

Due to the size limits that have been placed on the amount of data that can be transferred between a remote Exchange 2013 server and a local computer, not all cmdlets that once supported importing support this method of data transfer. To determine whether a specific cmdlet supports this method, see the Help information for the specific cmdlet.

These limits should accommodate the majority of typical operations that can be performed on an Exchange 2013 server. If the limits are lowered, you may find that some normal operations fail because they exceed the new limits. If the limits are raised, the data being transferred could take longer to transfer and become more at risk to transient conditions that interrupt the data transfer. Also, you may exhaust the memory on the remote server if you haven't installed enough memory to allow the server to store the entire block of data during transfer. Each possibility could result in data loss and therefore we recommend you don't change the default limits.

## Exporting files in remote Shell

The syntax to export files in Exchange 2013 is used any time you want to accept data from a cmdlet running on a remote Exchange 2013 server and store the data on your local computer or server. Cmdlets that provide data that you can save to a local file will output an object that will contain the **FileData** property (or something similar). Depending on the cmdlet, the **FileData** property is only populated on the object that's output in specific situations. To determine the correct property to use and when it can be used, see the Help information for the cmdlet you're using.

The Shell must know that you want to save the data stored in the **FileData** property to your local computer. To do so, use the following syntax.

```powershell
<cmdlet> | ForEach {<cmdlet> | ForEach {$_.FileData | Add-Content <local path to file> -Encoding Byte}.FileData | Add-Content <local path to file> -Encoding Byte}
```

For example, the following command exports the data stored in the **FileData** property on the object created by the **Export-SomeData** fictional cmdlet. The exported data is stored in a file you specify on the local computer, in this case MyData.dat.

> [!NOTE]
> This procedure uses the <STRONG>ForEach</STRONG> cmdlet, objects, and pipelining. For more information about each, see <A href="https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines">Pipelining</A> and <A href="https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_objects">Structured data</A>.

```powershell
Export-SomeData | ForEach {Export-SomeData | ForEach {$_.FileData | Add-Content C:\MyData.dat -Encoding Byte}.FileData | Add-Content C:\MyData.dat -Encoding Byte}
```

The following actions occur when the command is run:

1. The command is accepted by remote Shell.

2. Remote Shell calls the **Export-SomeData** cmdlet on the remote Exchange 2013 server.

3. The output object created by the **Export-SomeData** cmdlet is passed back to the local Shell session via the pipeline.

4. The output object is then piped to the **ForEach** cmdlet, which has a script block.

5. Within the script block, the **FileData** property on the current object in the pipeline is accessed. The data contained within the **FileData** property is piped to the **Add-Content** cmdlet.

6. The **Add-Content** cmdlet saves the data piped from the **FileData** property to the file MyData.dat on the local file system.

For specific information about how to export data from Exchange 2013, see the Help topics for the feature you're managing.
