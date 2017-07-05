```
Sub SaveAs()
    On Error Resume Next
    Dim FolderPath As String, FolderName As String, BN As String
    Dim ReturnValue As Integer

    BN = ActiveWorkbook.Name

    FolderPath = ThisWorkbook.Path
    FolderName = Mid(BN, 1, InStrRev(BN, ".", Len(BN)) - 1)

    Dim MyFile As Object
    Set MyFile = CreateObject("Scripting.FileSystemObject")

    If MyFile.folderexists(FolderPath & "\" & FolderName & "-Saved") Then
        ReturnValue = MsgBox("文件夹已存在，是否更新内容？", vbOKCancel, "Caution!")
        If ReturnValue = 2 Then Exit Sub
    Else
        MyFile.CreateFolder (FolderPath & "\" & FolderName & "-Saved")
        Set MyFile = Nothing
    End If

    Application.ScreenUpdating = False
    Application.DisplayAlerts = False

    Dim i As Integer

    For i = 1 To Sheets.Count
        Set Wk = Workbooks.Add
        Workbooks(BN).Sheets(i).Copy before:=Wk.Worksheets("Sheet1")
        Wk.SaveAs FolderPath & "\" & FolderName & "-Saved\" & ThisWorkbook.Sheets(i).Name
        Wk.Close
    Next i

    Application.DisplayAlerts = True
    Application.ScreenUpdating = True
End Sub
```