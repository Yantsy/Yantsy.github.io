### 会用到的类

std::string  
std::string_view  
std::vector  

### 字符串相关操作
字符串可以被视作字符的顺序容器，要实现字符串的split，首先需要把整个字符串看一遍，同时进行以下操作：  

1.查询分隔字符或者分隔字符串起始字符的位置。 
```cpp
size_type find( const basic_string& str, size_type pos = 0 ) const;//用于查询字符串中从pos开始查询到的分隔字符的位置
```

2.截取特定区间的字符；
```cpp
basic_string substr( size_type pos = 0, size_type count = npos ) const;//用于截取从pos开始npos个字符数的字符
```

### 最终实现

```cpp
auto split(std::string_view s, char delim) {
    std::vector<std::string> output;
    size_t begin { 0 };
    while (true) {
        size_t pause = s.find(delim, begin);
        if (pause == std::string::npos) {
            output.emplace_back(s.substr(begin));
            break;
        }
        output.emplace_back(s.substr(begin, pause - begin));//要注意，截取的字符数量应该是pause和begin之间字符的数量，所以是pause-begin
        begin = pause + 1;//delim就是一个字符，要从delim的后一位继续查询，所以得+1
    }
    return output;
}
auto split(std::string& s, std::string delim) {
    std::vector<std::string> output;
    size_t begin { 0 };
    while (true) {
        size_t pause = s.find(delim, begin);
        if (pause == std::string::npos) {
            output.emplace_back(s.substr(begin));
            break;
        }
        output.emplace_back(s.substr(begin, pause - begin));
        begin = pause + delim.length();//delim是字符串，要从间隔该字符串长度后的位置继续查询，所以+delim.length()
    }
    return output;
}
```