# 在python代码中直接调用共享库函数的方法
在UNIX c/c++中, 可以用dlopen()函数打开一个so共享库, 并调用其它配套的函数来获取里面的符号, 调用其中的函数等. python脚本中, 可以用同样的方法做同样的事情.  
在python中, 打开so库、获取函数（符号）、调用函数、关闭so库等活儿，已经被一个叫“ctypes”的python模块给承包了，我们直接用就行。

## 一个简单的demo  
以下是一个在python代码中调用so库的demo。  
python脚本代码如下：  

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
'''
    @brief: 演示在python中使用so共享库的方法
'''
import ctypes

# mymath = ctypes.cdll.LoadLibrary("mymath/libmymath.so")
# or:
mymath = ctypes.CDLL("mymath/libmymath.so")
lhs = 10
rhs = 2

print("%d + %d = %d" % (lhs, rhs, mymath.add(lhs, rhs)))
print("%d - %d = %d" % (lhs, rhs, mymath.sub(lhs, rhs)))
print("%d * %d = %d" % (lhs, rhs, mymath.muti(lhs, rhs)))
print("%d / %d = %d" % (lhs, rhs, mymath.div(lhs, rhs)))
```
代码中的"mymath/libmymath.so"库是一个用C语言编写的示例共享库，提供了以下接口：
```C
/*
 * @brief: 加法
*/
int add(int lhs, int rhs);

/*
 * @brief: 减法
*/
int sub(int lhs, int rhs);

/*
 * @brief: 乘法
*/
int muti(int lhs, int rhs);

/*
 * @brief: 除法
*/
int div(int lhs, int rhs);
```  
运行结果如下：　　
```shell
[Rain@DragonBook:python_so]$ ./demo_python_so.py 
10 + 2 = 12
10 - 2 = 8
10 * 2 = 20
10 / 2 = 5
```

## 函数参数
python中的数据类型与C中的数据类型并不是一一对应的，所以在python脚本中调用SO库的函数时函数的参数可能会遇到麻烦，如一个函数要求输入的参数是一个用户自定义结构体时，就无法直接将python的class直接输入到C函数中。只有None, integers, bytes objects and (unicode) strings这几种数据类型可以不用转换，它们的对应关系如下:  

|python   |C/C++                 |
|:--------|:---------------------|
|None     |NULL                  |
|bytes    |char \* or wchar_t \* |
|strings  |char \* or wchar_t \* |
|integers |int                   |

其它的数据类型需要转换一下，ctypes 中定义了一些与C对应的基本数据类型：

|ctypes type       |C type        |Python type        |
|:-----------------|:-------------|:------------------|
|c_bool            |_Bool         |bool (1)           |
|c_char            |char          |1-character bytes object|
|c_wchar           |wchar_t       |1-character string|
|c_byte            |char          |int               |
|c_ubyt            |unsigned char |int               |
|c_short|short|int|
|c_ushort|unsigned short|int|
|c_int|int|int|
|c_uint|unsigned int|int|
|c_long|long|int|
|c_ulong|unsigned long|int|
|c_longlong|__int64 or long long|int|
|c_ulonglong|unsigned __int64 or unsigned long long|int|
|c_size_t|size_t|int|
|c_ssize_t|ssize_t or Py_ssize_t|int|
|c_float|float|float|
|c_double|double|float|
|c_longdouble|long double|float|
|c_char_p|char * (NUL terminated)|bytes object or None|
|c_wchar_p|wchar_t * (NUL terminated)|string or None|
|c_void_p|void *|int or None|

示例：
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import ctypes

def wrap_param_demo():
    '''
        @brief: 演示python与C数据类型的转换
    '''
    libc = ctypes.CDLL("libc.so.6")
    printf = libc.printf

    printf(b"Hello, %s\n", b"World") # bytes, no wrap
    printf(b"Hello, %S\n", "World") # string, no wrap
    printf(b"Hello, %d\n", 2020) # integer, no wrap

    # error, we have to wrap double into ctypes type:
    #printf(b"%f yuan for a bottle of beer\n", 20.5)
    
    # correct, wrap double type into c_double type:
    # printf(b"%f yuan for a bottle of beer\n", ctypes.c_double(20.5))

if __name__ == "__main__":
    wrap_param_demo()
```

输出结果如下：

```shell
root@longcyVM:/media/sf_Studio/python_so# ./demo_python_so.py 
Hello, World
Hello, World
Hello, 2020
20.500000 yuan for a bottle of beer
```

另一种方法是指定函数的参数类型。函数有一个属性"argtypes"，用来指定函数输入的参数的类型，如：
```python
def specify_argument_types():
    '''
        @brief: 演示指定函数参数类型的方法
    '''
    libc = ctypes.CDLL("libc.so.6")
    printf = libc.printf
    printf.argtypes = [
        ctypes.c_char_p,
        ctypes.c_char_p,
        ctypes.c_int,
        ctypes.c_double]
    
    # correct:
    printf(b"String '%s', Int %d, Double %f\n", b"Hi", 10, 2.2)
    
    # error, argument types of printf has been modified:
    printf(b"%d %d %d", 1, 2, 3)

if __name__ == "__main__":
    specify_argument_types()
```

输出结果如下：

```shell
root@longcyVM:/media/sf_Studio/python_so# ./demo_python_so.py 
String 'Hi', Int 10, Double 2.200000
Traceback (most recent call last):
  File "./demo_python_so.py", line 61, in <module>
    specify_argument_types()
  File "./demo_python_so.py", line 56, in specify_argument_types
    printf(b"%d %d %d", 1, 2, 3)
ctypes.ArgumentError: argument 2: <class 'TypeError'>: wrong type

```

## 函数返回值
python中，默认所有的C函数返回值都是int类型的，如
```python
def return_types_demo():
    '''
        @brief: 演示如何指定返回值的类型
    '''
    libc = ctypes.CDLL("libc.so.6")
    strchr = libc.strchr

    # By default functions are assumed to return the C int type:
    ret = strchr(b"abcdef", ord("d"))
    print("return %d(type: %s)" % (ret, type(ret)))

if __name__ == "__main__":
    return_types_demo()
```

输出结果如下：

```shell
root@longcyVM:/media/sf_Studio/python_so# ./demo_python_so.py 
return 216169603(type: <class 'int'>)
```

从前一节得知，一个函数的参数类型可以用"argtypes"属性来指定，那么一个函数的返回值用什么来指定呢？答案是"restype"，如下所示：
```python
def return_types_demo():
    '''
        @brief: 演示如何指定返回值的类型
    '''
    libc = ctypes.CDLL("libc.so.6")
    strchr = libc.strchr

    strchr.restype = ctypes.c_char_p # c_char_p is a pointer to a string
    ret = strchr(b"abcdef", ord("d"))
    print("return %s(type: %s)" % (ret, type(ret)))

if __name__ == "__main__":
    return_types_demo()
```

输出结果如下：

```shell
root@longcyVM:/media/sf_Studio/python_so# ./demo_python_so.py 
return b'def'(type: <class 'bytes'>)
```
