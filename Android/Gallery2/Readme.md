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
                STMFD           SP!, {R11,LR}       ; 保存寄存器及返回值到堆栈
                LDR             R12, [SP,#8+arg_0]  ; 把参数5(int off)保存到R12中 
                ADD             LR, R3, R12         ; LR也就是参数4指向的地址加上偏移地址(&img[off])
                STRB            R2, [LR,#2]         ; R2(参数3)保存到[LR+2]所在的内存   //img[off+2] = b
                STRB            R1, [LR,#1]         ; R1(参数2)保存到[LR+1]所在的内存   //img[off+1] = g
                STRB            R0, [R3,R12]        ; R0(参数1)保存到[R3+R12]所在的内存 //img[off] = r
                LDMFD           SP!, {R11,PC}       ; 恢复寄存器，返回调用这个函数的代码继续运行
; End of function stuff
```


>R14 (the Link Register) holds the return address from a subroutine entered when you use the
branch with link ( BL ) instruction.

R14(LR)寄存器保存着当你在一个函数中使用BL指令跳转时的返回地址。

>R15 is the program counter and holds the current program address (actually, it always points
eight bytes ahead of the current instruction in ARM state and four bytes ahead of the current
instruction in Thumb state, a legacy of the three stage pipeline of the original ARM1 processor).
When R15 is read in ARM state, bits [1:0] are zero and bits [31:2] contain the PC. In Thumb
state, bit [0] always reads as zero.

R15(PC)保存着当前程序地址（事实上，它在ARM状态下总是纸箱当前指令后8个字节，在Thumb状态指向当前指令后4个字节）

arg_0大概代表参数5在堆栈中的偏移地址，在这里也就是0。
