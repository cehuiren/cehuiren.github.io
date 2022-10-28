---
title: Read a CSV Text File with MicroStation VBA
author: la
date: 2018-03-06 10:16:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 文本文件 读写 csv
excerpt: 
---
* content
{:toc}

Questions similar to this appear on the [Bentley Discussion Groups](https://communities.bentley.com/products/programming/microstation_programming/f/microstation-programming---forum). These problems appeared in the VBA discussion group.

- How do I open a text file with VBA?
- How do I read a text file with VBA?
- How do I read lines of CSV from a text file with VBA?
- How do I create a Point3d with VBA?

### Open a Text File
If you want to open a file interactively (i.e. ask the user to choose a file to open) then you need to use the Microsoft Common Dialogs. This function wraps it up for you. The code is not complete — there are some data structures (UDTs) that must be defined for this to work. Those UDTs are define in the VBA project that you can [download](http://www.la-solutions.co.uk/content/connect/MVBA/MVBA-ParseCsvFile.htm#Download).

```vb
' ---------------------------------------------------------------------
'   ShowOpen    Open common dialog
'   Returns:    full path of file to open, or zero-length string if Cancel
'   Example:
'   StrFilter = "Text Files (*.txt)" + Chr$(0) + "*.txt" + Chr$(0)
'   StrFilter = StrFilter & "All Files (*.*)" + Chr$(0) + "*.*" + Chr$(0)
' ---------------------------------------------------------------------
Public Function ShowOpen( _
    strTitle As String, _
    Optional strFilterDescr As String = "All files (*.*)", _
    Optional strFilterSpec As String = "*.*", _
    Optional strInitDir As String = vbNullString) As String
    On Error GoTo Proc_Error

    Dim OFName          As OPENFILENAME
    Dim strFileFilter   As String, _
        strFileSelected As String

    strFileFilter = strFilterDescr & Chr$(0) & strFilterSpec & Chr$(0)
    With OFName
        .lStructSize = LenB(OFName)
        .hwndOwner = 0&
        .hInstance = 0& 'App.hInstance
        'Select a filter
        .lpstrFilter = strFileFilter
        .lpstrFile = VBA.Space$(254)
        .nMaxFile = 255
        .lpstrFileTitle = VBA.Space$(254)
        .nMaxFileTitle = 255
        If (vbNullString <> strInitDir) Then _
            .lpstrInitialDir = strInitDir
        .lpstrTitle = strTitle ' "Select File"
        .flags = OFN_PATHMUSTEXIST Or OFN_FILEMUSTEXIST
    End With
    If GetOpenFileName(OFName) Then
        strFileSelected = Trim$(OFName.lpstrFile)
        If (InStr(strFileSelected, Chr(0)) > 0) Then
            strFileSelected = Left(strFileSelected, InStr(strFileSelected, Chr(0)) - 1)
        End If
        ShowOpen = Trim$(strFileSelected)
    Else
        ShowOpen = vbNullString
    End If

Proc_Exit:
    Exit Function
Proc_Error:
    ShowOpen = vbNullString
    MsgBox Err.Description
    Resume Proc_Exit
End Function
```

Here's the code to invoke the FileOpen function …
```vb
  Dim filePath As String
  filePath = ShowOpen("Open CSV File", "CSV Files (*.csv)", "*.csv")
  Debug.Print "You chose file " & Quote(filePath)
```

### Microsoft Scripting Runtime
The code in the downloadable project reference a Microsoft library named [Scripting Runtime](https://support.microsoft.com/en-gb/help/186118/how-to-use-filesystemobject-with-visual-basic). That library is almost certainly already installed on your computer.

Your VBA project needs a reference to that library. You can see libraries used by your project using the Tools|References menu (those are VBA reference, not MicroStation reference attachments).

![](/img/2022/2022-10-28-10-16-09.png)
_Microsoft Scripting Library_

If you [download the project](http://www.la-solutions.co.uk/content/connect/MVBA/MVBA-ParseCsvFile.htm#Download) you will find that the Scripting libary is already attached.

### Read Lines from a Text File
A  Microsoft Scripting makes it easy to open a text file …
```vb
  Dim oFile As Scripting.TextStream
  Set oFile = OpenFileForReading(filePath)
' ---------------------------------------------------------------------
'   Open the specified file for reading.
'   Returns:    Reference to a Scripting.TextStream object
' ---------------------------------------------------------------------
Public Function OpenFileForReading(ByVal filePath As String) As Scripting.TextStream
    Set OpenFileForReading = Nothing
    Debug.Print "Attempt to open " & Quote(filePath)
    Dim oFileSystem As New Scripting.FileSystemObject
    Set OpenFileForReading = oFileSystem.OpenTextFile(filePath, ForReading)
End Function
```

With the file now open, you can read lines of text. The loop below continues to read lines until the file is empty …

```vb
Dim line As String
Dim point As Point3d
Do Until oFile.AtEndOfStream
  line = oFile.ReadLine
  Debug.Print "line: '" & line & "'"
Loop
```

Parse Values from a Line of Text
The text file has multiple lines, each of which conveys several values. The values are separated by something like a comma, tab or space. For example, here's a line that uses a space character to separate numerals …

```
3.1 4.7 1.0
```

If we know the separator character (in this example, a space), we can split the line into an array of words …

```vb
Const Separator  As String = " "
Dim values() As String
values = Split(line, Separator)
```

Each member of the values array contains part of the line. In this example …

- values(0) = 3.1
- values(1) = 4.7
- values(2) = 1.0

Now we are in position to fill a Point3d with the parsed values. MicroStation VBA supplies the function Point3dFromXYZ, which fits our requirement exactly …

```vb
Dim point As Point3d
point = Point3dFromXYZ(CDbl(values(0)), CDbl(values(1)), CDbl(values(2)))
```

![](/img/2022/2022-10-28-10-16-34.png)

[Download Example Project](http://www.la-solutions.co.uk/content/connect/MVBA/code-samples/CsvPointReader.zip)

You can download the example [CSV File Parser Project](http://www.la-solutions.co.uk/content/connect/MVBA/code-samples/CsvPointReader.zip).

Copy the **.mvba** file to a known good location where MicroStation looks for VBA macros. For example, `..\Organization\Standards\Macros`{: .filepath } or any folder that configuration variable **MS_VBASEARCHDIRECTORIES** includes.

Open the project in the VBA editor. Read the notes in module **modMain**. That module also contains the keyin to run the example …

**vba run [CsvFileParser]modMain.Main**