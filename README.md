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
	
	EAX    eax是累加器，在做加法和乘法运算的时候，eax是默认寄存器    
    	EBX    ebx是基地址寄存器，通常被用于寻址    
	ECX    ecx是计数器，通常用于循环计数    
	EDX    edx通常用于存放除法运算产生的余数    
	ESI     esi是源寄存器，通常在字符串操作中使用
	EDI    edi与esi对应，edi表示目标寄存器    
	EBP    ebp是基址指针，通常用于指向栈底    
	ESP    esp是堆栈指针，与ebp对应，通常指向栈顶
        
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
#### 栈溢出

#### 堆溢出

#### 整数型溢出

#### UAF
```
#include <stdio.h>
#include <cstdlib>
#include <string.h>
int main()
{
    char *p1;
    p1 = (char *) malloc(sizeof(char)*10);//申请内存空间
    memcpy(p1,"hello",10);
    printf("p1 addr:%x,%s\n",p1,p1);
    free(p1);//释放内存空间
    char *p2;
    p2 = (char *)malloc(sizeof(char)*10);//二次申请内存空间，与第一次大小相同，申请到了同一块内存
    memcpy(p1,"world",10);//对内存进行修改
    printf("p2 addr:%x,%s\n",p2,p1);//验证
    return 0;
}
```
#### HOOk技术相关
```
	Hook技术分类：
	1.修改数据，通常指引用函数的地址（Address Hook）
	2.修改函数指令Hook(Inline Hook)

	两种hook方法 举例：

	#include "stdio.h"

	void Printchar(char *pch)
	{
		printf("address = 0x%x char = %c\n", pch, *pch);
	}

	int main(int argc, char* argv[])
	{
		char ch1 = 'a';
		char ch2 = 'b';
		char *Pchar;

	        Pchar = &ch1;
		Printchar(Pchar); //正常使用

		Pchar = &ch2;
		Printchar(Pchar); //address hook,修改地址

		Pchar = &ch1;//恢复初值

		*Pchar = 'b'; // Inline hook地址不变，修改内容

		Printchar(Pchar);

		return 0;

	}

```

#### 样本分析
```
1. windows常见木马api介绍

进程列表读取


2.IDA,OD,winDBG使用
```







