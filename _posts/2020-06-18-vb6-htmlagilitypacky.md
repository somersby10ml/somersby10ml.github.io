---
title: "using HtmlAgilityPack in vb6"
categories:
  - vb6

---

1. Download HtmlAgilityPack.dll with Nuget (or find dll from local)
1. Convert dll to tlb using **regasm**

    ```
    regasm -tlb -codebase dllPath
    regasm -tlb -codebase C:\Users\root\.nuget\packages\htmlagilitypack\1.11.17\lib\Net45\HtmlAgilityPack.dll
    ```

1. Create a project and reference `HtmlAgilityPack.tlb`.


```vb
Option Explicit
 
Private Sub Form_Load()
    Dim WinHttp As New WinHttpRequest
    WinHttp.Open "GET", "https://www.naver.com/"
    WinHttp.Send
 
    Dim mydoc As New HtmlAgilityPack.HtmlDocument
    mydoc.LoadHtml WinHttp.ResponseText
    
    Dim NodeCollection As HtmlNodeCollection
    Set NodeCollection = mydoc.DocumentNode.SelectNodes("//a")
    
    Dim i As Long
    For i = 0 To NodeCollection.Count - 1
        Dim Node As HtmlNode
        Set Node = NodeCollection(i)
        List1.AddItem Trim$(Node.InnerText) & " / " & Node.GetAttributeValue("href", "")
    Next i
End Sub
```