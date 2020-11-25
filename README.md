# Reverse-pwn
0x01 基础汇编知识（主要x86汇编演示）
```
    1.一个程序内部主要分为数据，代码，栈，堆
    2.x86简介
        x86寄存器
        通用寄存器：eax(ax,ah,al，其中ax 16位，ah高八位，al低8位），ebx，ecx，edx，ebp，esi
        段寄存器：cs,ss,ds,es,fs,gs
        标志寄存器：EFLAGS
        指令指针：EIP
        
        汇编指令
        MOV,LEA,ADD/SUB,PUSH,POP,CMP,JMP,J[condition],CALL,LEAVE,RET,.....
        
        指令格式
        助记符——————目的操作数——————源操作数
        mov         ecx            0x1234
        
        两种格式intel和AT&T
        intel ： mov eax, 8
        AT&T ： movl $8, %eax

    3.函数调用栈   
```
