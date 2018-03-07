# 对照源码学习ARM下的汇编


首先看一个最简单的函数，此函数位于`jni/filters/redEyeMath.c`。

```c
void stuff(int r, int g, int b, unsigned char *img, int off) {
    img[off + 2] = b;
    img[off + 1] = g;
    img[off] = r;
}
```

用IDA反汇编后是这样的：

```asm
 EXPORT stuff
stuff                                   ; DATA XREF: LOAD:000007F8↑o

arg_0           =  0

; __unwind {
                STMFD           SP!, {R11,LR}       ; 保存寄存器值及返回地址到堆栈，同时SP减8(存入堆栈两个值)
                LDR             R12, [SP,#8+arg_0]  ; R12 = *[SP+8+arg_0]，也就是参数5?? arg_0到底什么意思？
                ADD             LR, R3, R12         ; LR = R3+R12，也就是参数4所指向的内存地址再加上参数5(偏移地址)
                STRB            R2, [LR,#2]         ; R2(参数3)保存到[LR+2]所在的内存
                STRB            R1, [LR,#1]         ; R1(参数2)保存到[LR+1]所在的内存
                STRB            R0, [R3,R12]        ; R0(参数1)保存到[R3+R12]所在的内存
                LDMFD           SP!, {R11,PC}       ; restore regs and return
; End of function stuff
```




```c
void __fastcall stuff(int r, int g, int b, char *img, int off)
{
  char *result; // lr

  result = &img[off];
  result[2] = b;
  result[1] = g;
  *result = r;
}
```





