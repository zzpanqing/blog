* __declspec(属性) 放在 class, function, variable 的前面，将属性应用给 class, function, variable

## import / export class from / into .dll ##
* __declspec(dllexport) 
    
    将 class, function, variable 放在 dll 中。
    
    在要输出的class, function, variable 的声明前加上_declspec(dllexport)的修饰符，表示输出。
    
*  __declspec(dllimport)
   
   外部程序可以引用 class 的 static 成员和 static 成员函数。
 
 
一般一个 class 要这样写，才能保证 class 所有的 name (class, function, variable) 都被　export  到 .dll   中，并且能从 .dll  中被 import 到外部程序


SimpleDLLClass.h

    #ifdef SIMPLEDLL_EXPORT
      #define DLL_EXPORT __declspec(dllexport)
    #else
      #define DLL_EXPORT __declspec(dllimport)
    #endif

    class DLL_EXPORT SimpleDLLClass
    {
    public:
      SimpleDLLClass();
      virtual ~SimpleDLLClass();
      virtual getValue() { return m_nValue;};
    private:
      static int m_nValue;
    };


SimpleDLLClass.cpp


    #include "SimpleDLLClass.h"

    SimpleDLLClass::SimpleDLLClass()
    {
      m_nValue=0;
    }

    SimpleDLLClass::~SimpleDLLClass()
    {
    }

## export extern global variable ## 
[来源](http://stackoverflow.com/questions/19373061/what-happens-to-global-and-static-variables-in-a-shared-library-when-it-is-dynam) 
###背景###
一般的  global variable 是不被 dll 外的 module 可见的, 就是说 exe 看不见 dll 中的 extern glable variable. 原因
extern global variable 不是exported symbols。

你的程序试图访问 extern global variable link 出错。要程序通过link，必须 static link 提供该 extern variable 的 .obj 或者.lib.
问题来了:
你有两个版本的  extern variable, 一个属于 .exe, 一个属于 .dll

###windows 下 如何 export a global variable ###

    #ifdef COMPILING_THE_DLL
        #define MY_DLL_EXPORT extern "C" __declspec(dllexport)
    #else
        #define MY_DLL_EXPORT extern "C" __declspec(dllimport)
    #endif

    MY_DLL_EXPORT int my_global;
    
这样 dll 和 exe 用的就是同一个 my_global 了

###Unix-like 系统下完全不是问题###

Unix-like 系统下动态链接库叫.so (shared objects). Unix-like 系统 export 所有的 global variable 和 functions
. 所有的 global variable 和 functions 都是 shared.
