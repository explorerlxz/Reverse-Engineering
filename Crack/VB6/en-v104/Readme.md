# 去除限制


Tools: VB Decompiler Pro 10.0

## Patch Sub_Main file


```
  loc_5E6648: arg_8(6596) = Unknown_5C891C(var_AC, var_A4)
  loc_5E6652: arg_8(6596) = Unknown_5D05FC(var_AC)
  loc_5E665D: If Not(arg_8(6596)) Then '5E6662
  loc_5E6660:   End
  loc_5E6662: End If
loc_5E6667: ChDrive arg_8(3256)
```

Patch address 0x5e6648-0x5e6661 to 0x00





## Patch Frm_Explorer → Form_load()

```
loc_5D82F0: Call Proc_13_49_5F2890()
loc_5D82F5: Proc_2_55_601968()

loc_5D8306: var_A8 = Me.Global.Screen
```

Patch 0x5D82F0-0x 5D82F9 to 0x00

如果修改到0x5d8306会出错！！


## Patch Frm_Explorer → Timer1_Timer()

```
loc_5C1847:   If Not(Proc_2_53_5D05FC(0)) Then
loc_5C184A:     End
loc_5C184C:   End If

loc_5C184C: End If
```

Patch 0x5C1847-0X5C184B//出错

Patch 0x5C1840-0X5C184B//还是有问题，说明还有其它地方调用Proc_2_53_5D05FC(0)

