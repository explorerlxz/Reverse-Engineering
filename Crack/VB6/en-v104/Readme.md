# 去除程序加密限制


破解工具: VB Decompiler Pro 10.0

## Patch Mdl_Main中的Proc_2_0_5E6920


```
  loc_5E6624: var_AC = GetSystemDefaultLCID(var_8A, var_88, StrConv(var_A8, vbUnicode))
  loc_5E662E: var_A4 = "HH:mm:ss"
  loc_5E663A: SetLocaleInfo(var_AC, &H1003, var_A4)
  loc_5E6648: Me(6596) = Proc_2_56_5C891C(var_AC, var_A4)
  loc_5E6652: Me(6596) = Proc_2_53_5D05FC(var_AC)
  loc_5E665D: If Not(Me(6596)) Then
  loc_5E6660:   End
  loc_5E6662: End If
  loc_5E6667: ChDrive Me(3256)
  loc_5E6671: ChDir Me(3256)
  loc_5E6676: Proc_2_31_5CE4F8()
  loc_5E668F: var_A0 = Me.Global.Screen
```


### 方法一

修改0x5e6648 ~ 0x5e6661处的内容为0x00。


### 方法二

修改0x5e6660处的0xfcc8为0x0000，也就是`loc_5E6660: End`这一行。





## Patch Frm_Explorer中的 Form_load()

```
  loc_5D82CB: Call Proc_13_38_5EA6C4()
  loc_5D82D5: Proc_10_0_5C1934(0)
  loc_5D82E3: If (unk_411250.global_40 = 3) Then
  loc_5D82EB:   Proc_14_5_5C2970(1)
  loc_5D82F0: End If
  loc_5D82F0: Call Proc_13_49_5F2890()
  loc_5D82F5: Proc_2_55_601968()
  loc_5D8306: var_A8 = Me.Global.Screen
  loc_5D830E: Screen.MousePointer = var_A2
```

### 方法一

Patch 0x5D82F0-0x 5D82F9 to 0x00

如果修改到0x5d8306会出错！！原因不明


### 方法二

修改Mdl_Main中的`Proc_2_55_601968()`子程序的异常处理代码


```
  loc_6009BC: On Error Goto loc_601963
  loc_6009C6: For var_2B4 = 0 To 2: var_86 = var_2B4 'Integer
  loc_6009DD:   var_AC(CLng(var_86)) = CLng((&HE + (var_86 * 6)))
  loc_6009EA:   var_C8(CLng(var_86)) = 6
……
……
……
  loc_601937:   If (var_2AC(2) <= 0) Then
  loc_60195A:     var_22C = DogWrite(6, 8, "S911bW", var_2BC, 1, var_240)
  loc_601960:     End
  loc_601962:     ' Referenced from: 601925
  loc_601962:   End If
  loc_601962: End If
  loc_601962: Exit Sub
  loc_601963: ' Referenced from: 6009BC
  loc_601963: End
  loc_601965: Exit Sub
```


修改0x601963处的0xfcc8为0x0000，也就是`loc_601963`这行的`End`指令。

## Patch Frm_Explorer → Timer1_Timer()

```
Private Sub Timer1_Timer() '5C1850
  'Data Table: 4111C4
  Dim MemVar_608D18.global_198 As Integer
  loc_5C1820: MemVar_608D18.global_198 = (MemVar_608D18.global_198 + 1)
  loc_5C182E: If (MemVar_608D18.global_198 >= &H3C) Then
  loc_5C1847:   If Not(Proc_2_53_5D05FC(0)) Then
  loc_5C184A:     End
  loc_5C184C:   End If
  loc_5C184C: End If
  loc_5C184C: Exit Sub
End Sub
```

修改0x5c184a处的0xfcc8为0x0000

## 总结

VB6中`End`对应的二进制代码可能是0xfcc8，所以最简单的修改方法为

```
修改0x5e6660处的0xfcc8为0x0000
修改0x601963处的0xfcc8为0x0000
修改0x5c184a处的0xfcc8为0x0000
然后把破解后用不到的加密软件和动态链接库删去。
```
