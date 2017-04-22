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
