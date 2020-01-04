---
layout: post
title: "Excel Notes"
date: 2019-05-01
categories: excel vba
---

## Macro to split rows into sheets

[Sourcelink](https://www.extendoffice.com/documents/excel/1175-excel-split-rows-into-multiple-worksheets.html#a1)

```vb
Sub SplitData()

Dim WorkRng As Range
Dim selectedRng As Range
Dim xRow As Range
Dim xHeader As Range
Dim SplitRow As Integer
Dim hasHeader As Boolean
Dim xWs As Worksheet
Dim xTitleId As String
Dim resizeCount As Integer
Dim i As Long
Dim destAddress As String

On Error Resume Next
xTitleId = "Split rows into Sheets"
Set selectedRng = Application.Selection
Set WorkRng = Application.InputBox("Range", xTitleId, selectedRng.Address, Type:=8)

If Not WorkRng Is Nothing Then
    SplitRow = Application.InputBox("Split Row Num", xTitleId, 500, Type:=1)
    If SplitRow <> 0 Then
        hasHeader = Application.InputBox("Has Header?", xTitleId, True, Type:=4)
        Set xWs = WorkRng.Parent
        Set xHeader = WorkRng.Rows(1)

        If hasHeader And WorkRng.Rows.Count > 1 Then
            Set xRow = WorkRng.Rows(2)
        Else
            Set xRow = WorkRng.Rows(1)
        End If

        Application.ScreenUpdating = False

        For i = 1 To WorkRng.Rows.Count Step SplitRow
            resizeCount = SplitRow
            If (WorkRng.Rows.Count - xRow.Row + 1) < SplitRow Then
                resizeCount = WorkRng.Rows.Count - xRow.Row + 1
            End If
            destAddress = "A1"
            Application.Worksheets.Add after:=Application.Worksheets(Application.Worksheets.Count)
            If hasHeader Then
                xHeader.Copy
                Application.ActiveSheet.Range("A1").PasteSpecial
                destAddress = "A2"
            End If
            xRow.Resize(resizeCount).Copy
            Application.ActiveSheet.Range(destAddress).PasteSpecial
            Set xRow = xRow.Offset(SplitRow)
        Next
    End If
End If
Application.CutCopyMode = False
Application.ScreenUpdating = True
End Sub
```

## Formula to conditional formating the cells for invalid data

[Source: Case in sensitive search](https://exceljet.net/formula/case-sensitive-match)

[Source: SUMPRODUCT formula](https://exceljet.net/excel-functions/excel-sumproduct-function)

[Source: Double Negative operator](https://exceljet.net/the-double-negative-in-excel-formulas)

```vb
=IF(NOT(ISBLANK(D2)), SUMPRODUCT((--EXACT(D2, Type)))=0)

'Tye is the named range pointing to the datalist
```

- The conditional formulas don't support array formulas when the conditional formatting added from vba code.
- EXACT returns the array with true and false. True for exact match
- Double negative operator converts true to 1 and false to 0
- SUMPRODUCT takes single/two arrays of equal size. When supplied with two arrays then it would first multiply the values and then sums them up. If single array is supplied then it just sums them up.
- The result is not array so we don't need to press alt enter and could be used in the conditional formula when added from the vba code.
