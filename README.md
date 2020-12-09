# Reverse-pwn
```
（本老菜想到哪里写到哪里，可能会遗漏和错误，不定时更新）
```
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

    3.函数调用栈 (重要！！！)  
    
     经典c代码片段如下：
	      intfunc_B(int arg_B1, int arg_B2)
	{
	 int var_B1, var_B2;
	 var_B1=arg_B1+arg_B2;
	 var_B2=arg_B1-arg_B2;
	 return var_B1*var_B2;
	}
	intfunc_A(int arg_A1, int arg_A2)
	{
	 int var_A;
	 var_A = func_B(arg_A1,arg_A2) + arg_A1 ;
	 return var_A;
	}
	int main(int argc, char **argv, char **envp)
	{
	 int var_main;
	 var_main=func_A(4,3); 
     
     
    
```
#### 格式化字符串

    漏洞原理
```
    程序中提供了参数可控（该格式化字符串参数来自外部输入）的printf族和scanf族函数或错误的参数类型或格式化字符串参数和传入参数个数不一致等情况，这导致我们可以控制程序行为或泄露一些信息
    
    printf函数为例来说明几个主要的格式化字符串的参数：
    %d - 十进制 - 输出十进制整数
    %s - 字符串 - 从内存中读取字符串
    %x - 十六进制 - 输出十六进制数
    %c - 字符 - 输出字符
    %p - 指针 - 指针地址
    %n – 把前面打印的字符长度输出到指定地址
    %N$ - 第N个参数

    1.参数完全可控

	#include <stdio.h>
	#include <string.h>

	int main (int argc, char *argv[])
	{
		char buff[1024];    // 设置栈空间

		strncpy(buff,argv[1],sizeof(buff)-1);
		printf(buff); //触发漏洞

		return 0;
	}
```
#### uaf


