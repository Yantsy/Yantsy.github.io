### From Source File to Executable File:

<img src="img/image.png" title="程序运行过程"/>

And previous to compilation,there is preprocessing ,in which stage the preprocessor directives like #include,#define,#ifdef,#ifndef,#endif.#ifdef...#endif ensures the codes between are compiled if some condition like a macro is defined.

### Functions

### Types,Variables and Arithmetic

A declaration is a statement that introduces an entity into the program and specifies its type。
> An object is some memory(RAM or CPU Register) that holds a value of some type
>A value is a set of bits interpreted according to a type
>A variable is a named object
>An instance is an initialize object

Assignment is the process of defining a variable and assigning a value to a it.

Initializtion is the process of specifying an initial value for an object.(copy=,direct(),direct-list{})

Fundamental type:bool,char,int,double,float,unsigned...

Each fundamental type corresponds directly to hardware facilities and has a fixed size that determines the range of values that can be stored in it.

A char variable is of the natural size to hold a character on a given machine (typically an 8-bit byte), and the sizes of other types are multiples of the size of a char.(sizeof(char)=1,and sizeof(int)=4).

