---
title: "exe to vbs vb6"
categories:
  - vb6

---

It was written in Visual Basic 6.  

The contents of the EXE are written to the VBS array.  
In VBS, the contents of the array are written to a **temporary folder** and executed.  


Text1 = Executable file path  
Text2 = Output vbs path  

```vb
Private Sub Command4_Click()    '## 변환
    If LenB(Dir$(Text2.Text)) Then
        Kill Text2.Text
    End If
    
    '## 파일의 바이너리를 가져옴
    Dim FileBinary() As Byte, FileSize As Long
    Open Text1.Text For Binary Access Read As #1
        FileSize = LOF(1)
        
        ReDim FileBinary(FileSize - 1) As Byte
        Get #1, , FileBinary()
    Close #1
    
    
    '## VBS (Visual Basic Script) 에서 쓰여질 변수이름(Variable Name)
    
    Const FileData As String = "VJr32hoir3o2c3y2io7OIRVrv23hrco3iuryvi"
    Dim FileSaveName As String
    FileSaveName = "KJVRh23crhiery23icri9vaSdhva" & FileSaveName
    
    Open Text2.Text For Output Access Write As #1
    
        Print #1, FileData; "="; """";
        
        Dim i As Long
        For i = LBound(FileBinary) To UBound(FileBinary)
            Print #1, Right$("0" & Hex(FileBinary(i)), 2);
        Next i
        
        Print #1, """"
        
        Print #1, "Private Sub SaveFile(fName, str)"
        Print #1, "Dim temp"
        Print #1, "Set xmldoc = CreateObject(""Microsoft.XMLDOM"")"
        Print #1, "xmldoc.loadXml ""<?xml version=""""1.0""""?>"""
        Print #1, "Set pic = xmldoc.createElement(""pic"")"
        Print #1, "pic.dataType = ""bin.hex"""
        Print #1, "pic.nodeTypedValue = str"
        Print #1, "temp = pic.nodeTypedValue"
        Print #1, "With CreateObject(""ADODB.Stream"")"
        Print #1, "    .Type = 1"
        Print #1, "    .open"
        Print #1, "    .write temp"
        Print #1, "    .SaveToFile fName, 2"
        Print #1, "    .Close:"
        Print #1, "    End With"
        Print #1, "End Sub"

        '## Main
        Print #1, "set ws = CreateObject(""WScript.Shell"")"
        Print #1, "fn = ws.ExpandEnvironmentStrings(""%temp%"") & ""\" & FileSaveName & """"
        Print #1, "saveFile fn , " & FileData
        'Execute ws.Exec fn
        Print #1, "ws.Exec fn"
        Print #1, "WScript.Sleep 301"
        Print #1, "WScript.Quit"
        
        
    Close #1
    
    'Shell "notepad.exe " & Text2.Text, vbNormalFocus
    MsgBox "성공", vbInformation, ""
End Sub
```