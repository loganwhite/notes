## How to remove the password protecting the sheet.

1. Right click the sheet tab and select `View Code`. This brings you to the VB window.
2. Copy and Paste the following code snippet.
```basic
Option Explicit
Sub GetPass()
    Const a = 65, b = 66, c = 32, d = 126
    Dim i#, j#, k#, l#, m#, n#, o#, p#, q#, r#, s#, t#
    With ActiveSheet
        If .ProtectContents Then
            On Error Resume Next
            For i = a To b
                For j = a To b
                    For k = a To b
                        For l = a To b
                            For m = a To b
                                For n = a To b
                                    For o = a To b
                                        For p = a To b
                                            For q = a To b
                                                For r = a To b
                                                    For s = a To b
                                                        For t = c To d
            .Unprotect Chr(i) & Chr(j) & Chr(k) & Chr(l) & Chr(m) & _
            Chr(n) & Chr(o) & Chr(p) & Chr(q) & Chr(r) & Chr(s) & Chr(t)
                                                        Next t
                                                    Next s
                                                Next r
                                            Next q
                                        Next p
                                    Next o
                                Next n
                            Next m
                        Next l
                    Next k
                Next j
            Next i
            MsgBox "Finished"
        End If
    End With
End Sub
```
3. Hit the run botton, and the script will remove the password automatically.
