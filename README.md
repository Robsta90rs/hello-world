# hello-world
Hello this is Rob
I am here to learn

Sub parse_data()
    Dim lr As Long
    Dim ws As Worksheet
    Dim vcol, i As Integer
    Dim icol As Long
    Dim myarr As Variant
    Dim title As String
    Dim titlerow As Integer
    vcol = 1
    Set ws = Sheets("Sheet1")
    lr = ws.Cells(ws.Rows.Count, vcol).End(xlUp).Row
    title = "A1:C1"
    titlerow = ws.Range(title).Cells(1).Row
    icol = ws.Columns.Count
    ws.Cells(1, icol) = "Unique"
    For i = 2 To lr
        On Error Resume Next
        If ws.Cells(i, vcol) <> "" And Application.WorksheetFunction.Match(ws.Cells(i, vcol), ws.Columns(icol), 0) = 0 Then
        ws.Cells(ws.Rows.Count, icol).End(xlUp).Offset(1) = ws.Cells(i, vcol)
        End If
    Next
    myarr = Application.WorksheetFunction.Transpose(ws.Columns(icol).SpecialCells(xlCellTypeConstants))
    ws.Columns(icol).Clear
    For i = 2 To UBound(myarr)
        ws.Range(title).AutoFilter field:=vcol, Criteria1:=myarr(i) & ""
        If Not Evaluate("=ISREF('" & myarr(i) & "'!A1)") Then
            Sheets.Add(after:=Worksheets(Worksheets.Count)).Name = myarr(i) & ""
        Else
            Sheets(myarr(i) & "").Move after:=Worksheets(Worksheets.Count)
        End If
        ws.Range("A" & titlerow & ":A" & lr).EntireRow.Copy Sheets(myarr(i) & "").Range("A1")
        Sheets(myarr(i) & "").Columns.AutoFit
    Next
    ws.AutoFilterMode = False
    ws.Activate
End Sub
