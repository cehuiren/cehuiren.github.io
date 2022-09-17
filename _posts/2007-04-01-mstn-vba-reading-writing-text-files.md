---
layout: post
title:  "READING AND WRITING TEXT FILES"
date: 2007-04-01
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 文本文件 文件读写 
excerpt: MicroStation VBA中文本文件读写常用代码。
author: 网络转载
---
* content
{:toc}

Being able to read from and write to text files in VBA is useful for the import/export of coordinate data as well as other file and configuration parameters.
### Opening Files
To work with a text file you will typically need to dimension three variables:
1. A String variable for the file name     `Dim txtFileName As String`
2. A second String variable for the line of text to read/write     `Dim txtFileLine As String`
3. A Long variable for a file number required by VBA     `Dim txtFileNumber As Long`
After the variables are dimensioned the next step is to find an available file number to use for opening the file and the required file I/O.  Use the VBA FreeFile function to assign the next available number.
`txtFileNumber = FreeFile`
Once you have an available file number you can open the file.

#### Open For Read
Use the For Input option of the open command to open a text file for reading. The file must exist prior to issuing the open statement or VBA will raise an error.  Checking for the existence of a file, and error handling will be discussed in future tips.
`Open txtFileName For Input As #txtFileNumber`

#### Open For Write
When opening a text file for writing you have two options. The For Output option will create a new file if it does not exist, or clear out the contents of the file if it does exist. The For Append option will also create a new file, but will add new lines on to the end of the file if it already exists.

`Open txtFileName For Output As #txtFileNumber`

`Open txtFileName For Append As #txtFileNumber`

## Reading from a text file
Use the following loop structure as a template for processing the entire file:
```vb
Do While Not EOF(txtFileNumber)
Line Input #txtFileNumber, txtFileLine
‘
‘ Do something with the text here
‘
Loop
```

Text files are accessed sequentially which means if you are only interested a particular line you will need to skip the lines you are not interested in to get to the line you a want. The following loop structure will help you accomplish this:

```vb
For i = 1 to linesToSkip
Line Input #txtFileNumber, txtFileLine
Next
Line Input #txtFileNumber, txtFileLine
‘
‘ Do something with the text here
‘
```

## Writing to a text file
Once you get your line of text formatted a simple print statement is all that’s needed to write to the file.
`Print #txtFileNumber, txtFileLine`

## Closing files
Once you are done interacting with the text file you will need to close it.
`    Close txtFileNumber`

## Putting it all together
The following subroutine illustrates one way to read text from an input file, modify the line, and then write it to an output file.

```vb
Sub TextFileIO_Example()
    Dim txtFileName_IN As String
    Dim txtFileNumber_IN As Long
    Dim txtFileLine_IN As String
    Dim txtFileName_OUT As String
    Dim txtFileNumber_OUT As Long
    Dim txtFileLine_OUT As String
    txtFileName_IN = "C:\Temp\TextFile_IN.txt"
    txtFileName_OUT = "C:\Temp\TextFile_OUT.txt"
    txtFileNumber_IN = FreeFile
    Open txtFileName_IN For Input As #txtFileNumber_IN
    txtFileNumber_OUT = FreeFile
    Open txtFileName_OUT For Output As #txtFileNumber_OUT
    Do While Not EOF(txtFileNumber_IN)
        Line Input #txtFileNumber_IN, txtFileLine_IN
        txtFileLine_OUT = "# " & txtFileLine_IN
        Print #txtFileNumber_OUT, txtFileLine_OUT
    Loop
    Close txtFileNumber_OUT
    Close txtFileNumber_IN
End Sub
```