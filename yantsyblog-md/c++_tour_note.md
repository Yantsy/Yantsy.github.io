### From Source File to Executable File:

<img src="img/image.png" title="程序运行过程"/>

在编译之前，还存在着一个叫做预处理的过程，这也是预处理器发挥作用的地方。

常见且常用的预处理器有#include（引入库）,#define（定义宏）,#ifdef（条件编译）,#ifndef,#endif.#ifdef。#include的作用是在预处理时将对应库的文件复制粘贴到它自身所在的位置。宏不检查语法，只负责替换（将代码中使用的宏替换为对应值）。条件编译能决定哪些代码被编译，常常被用于避免重复编译（和#pragma once作用相同），以及处理跨平台开发。

### Functions

### Types,Variables and Arithmetic

>A declaration is a statement that introduces an entity into the program and specifies its type。  
> An object is some memory(RAM or CPU Register) that holds a value of some type  
>A value is a set of bits interpreted according to a type  
>A variable is a named object  
>An instance is an initialize object

C++并不直接接触内存，而是通过object间接操控内存。

object是一块内存区域（RAM或者CPU寄存器），当object被赋予一个名字时，我们就得到了一个variable。

>Defining a variable:  
>int x; //类型+变量名（identifier）

>Assignment is the process of defining a variable and assigning a value to it.  
>int x =10;//copy assignment  
>int y =x ;//copy x to y,y=10

>Initializtion is the process of specifying an initial value for an object.(copy=,direct(),direct-list{})  
>int x =10;  
>int x(10);  
>int x{10};

从实用的角度考虑，我们应该尽量使用{}，因为这可以避免narrowing conversions，例如：

>int x=10.001;//10.001->narrowed to->10  
>int x{10.001};//error  
>float x{10.001};//correct

或者可以直接使用auto，这样便可以不判断类型，也无需担心narrowing conversions了。

常用的数据类型有：bool,char,int,double,float。值得注意的是：

>A char variable is of the natural size to hold a character on a given machine (typically an 8-bit byte), and the sizes of other types are multiples of > the size of a char.(sizeof(char)=1,and sizeof(int)=4).

>Logical operators:Bitwise(&AND,|OR,^XOR,~NOT,performed to each bit,output a number),Logical(&&AND,||OR,!NOT).Use the bitwise for certain caculations and the latter for if().

### Scope AND Lifetime

Local Scope,Class Scope,Namespace Scope
