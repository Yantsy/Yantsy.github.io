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

Object并不总有名字，比如temporaries and objects created using new（即指针）。

从实用的角度考虑，我们应该尽量使用{}，因为这可以避免narrowing conversions，例如：

>int x=10.001;//10.001->narrowed to->10  
>int x{10.001};//error  
>float x{10.001};//correct

或者可以直接使用auto，这样便可以不判断类型，也无需担心narrowing conversions了。

常用的数据类型有：bool,char,int,double,float。值得注意的是：

>A char variable is of the natural size to hold a character on a given machine (typically an 8-bit byte), and the sizes of other types are multiples of > the size of a char.(sizeof(char)=1,and sizeof(int)=4).

>Logical operators:Bitwise(&AND,|OR,^XOR,~NOT,performed to each bit,output a number),Logical(&&AND,||OR,!NOT).Use the bitwise for certain caculations and the latter for if().

### Scope AND Lifetime

主要有三类作用域，分别是：Local Scope,Class Scope,Namespace Scope。

Local Scope：在函数或者lambda中声明的变量，其作用于仅限于{}内，超出这个范围后会自动销毁。值得注意的是，通过new创建的指针也会销毁，但是它指向的object却并不会，因此必须手动释放这块内存（delete）。

Class Scope:在类中定义的变量，函数其作用于在于整个类，而不遵循先后顺序。

Namespace Scope:A name is called a namespace member name if it is defined in a namespace (§3.3) outside any function, lambda (§7.3.2), class (§2.2, §2.3, Chapter 5), or enum class (§2.4). Its scope extends from the point of declaration to the end of its namespace.

A name is called a namespace member name if it is defined in a namespace (§3.3) outside any function, lambda (§7.3.2), class (§2.2, §2.3, Chapter 5), or enum class (§2.4). Its scope extends from the point of declaration to the end of its namespace.

### const,constexpr,consteval

对于variable，const，constexpr能够确保该variable不会在其生命周期中再次被更改，但是前者允许在运行时得到variable的值，后者则要求variable的值在编译期间就能确定。

对于函数，constexpr,consteval都能够让函数在编译时就运算，但是前者能让函数既可以在编译时调用，也可以在运行时调用，而后者则强制函数在编译时完成计算。

此外，需要注意的是，由constexpr修饰的函数，其内部调用的函数也必须在编译时也能被调用，否则该函数也无法执行。例如：

```cpp
constexpr auto findPxFmt(const AVCodecContext *pcdCtx) const noexcept {

const auto *ppixFmt = av_get_pix_fmt_name(pcdCtx->pix_fmt);

if (ppixFmt == nullptr) {

std::cerr << "Can't find supported pixel format\n";

// exit(-1);

}

std::cout << "__pixel format:" << ppixFmt << "\n";

return pcdCtx->pix_fmt;

}
```
此时编译时会报错：

>warning: call to non-‘constexpr’ function ‘const char* av_get_pix_fmt_name(AVPixelFormat)’ [-Winvalid-constexpr]
   16 |     const auto *ppixFmt = av_get_pix_fmt_name(pcdCtx->pix_fmt);

原因在于[av_get_pix_fmt_name](https://ffmpeg.org/doxygen/1.1/pixdesc_8c.html#a925ef18d69c24c3be8c53d5a7dc0660e)必须要在程序运行时才可以执行。

还需注意的是const的位置，其在函数名前约束的是输出值在其生命周期内都不能被修改。

在()内则修饰的是函数内local scope内的变量，使传入的值在函数内无法被修改，如在findPxFmt中pcdCtx无法被修改，在（）后修饰的是成员变量,确保findPxFmt所属类的成员变量也无法在函数内被修改，二者共同使得findPxFmt只能读取pcdCtx而无法对其进行修改。

### Pointers,Arrays and References


