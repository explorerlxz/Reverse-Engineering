# 通过Gallery2的C++源码，提高对arm下汇编语言的逆向分析理解能力


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
                STMFD           SP!, {R11,LR}
                LDR             R12, [SP,#8+arg_0]
                ADD             LR, R3, R12
                STRB            R2, [LR,#2]
                STRB            R1, [LR,#1]
                STRB            R0, [R3,R12]
                LDMFD           SP!, {R11,PC}
; End of function stuff
```



