# 理解特殊成员函数的生成 #

在C++98，特殊成员函数有四个

* default constructor
* destructor
* copy constructor
* copy assignment operator

用到的时候才生成。

它们是 隐式public和inline的。

某个类继承了 destructor 是virutal 的基类，那么这个类的 destructor 是虚函数。

C++11 中，又添加两个新的，编译器可以自动生成的函数 move constructor 和 move assignment operator

    class Widget {
      public:
          ...
          Widget(Widget&& rhs);   // move constructor
                             `
          Widget& operator=(Widget&& rhs);   // move assign operator
          ..
      };
      
* move constructor 对 rhs 的 non-static member 逐一请求移动。
* move assignment operator 也将 rhs 的 non-static member 逐一请求移动。

* 请求移动是指，能移动就移动 （使用 std::move），不能移动就拷贝（不支持移动操作的类型，例如，大部分C++98的类）。通过重载函数决策来决定std::move表示为拷贝还是移动.

* 如果你生成了 move constructor 和 move assign operator，compiler 就不会替你生成。 

* move constructor 和 move assignment operator 相互 non independent, 如果你生成了其中一个， compiler 就 move constructor 和 move assignement constructor 都不生成。原因： compiler 生成的默认的 move constructor 和 move assignment operator 试图作 memberwise move.如果你生成了move constructor 或者move assignment operator 就说明你不需要 compiler 提供的memberwise move 这样的行为。    

* 如果你 declare 了 copy operation (copy constructor 和 copy assignment operator), 那么 compiler 也不会生成 move operation(move constructor 和 move assignment operator) 因为：声明 copy operation 说明你不想要默认的 member-wise copy 的 行为，那你也很可能不想要 move memberwise 的 行为 (默认 move operation 的行为)。


* 同样，声明move operation(member wise move 不被需要) 会导致 copy operation 无法被生成( member wise copy 也不需要)。

* Rule of Three
    
    * 如果你声明了 copy constructor, copy assignment operator, destructor 中的一个，那么你应该将三个都声明。原因，copy constructor 和 copy assignment operator 应该有一样的 resrouces 管理， destructor 也需要参与 resources 管理 (释放资源)。stl 中涉及内存的类 (stl container) 都声明了 copy constructor, copy assignment operator, destructor.
    
* 根据上面的道理，C++11 中，如果声明了 destructor, 则 compiler 不会生成 move operation (move constructor 和 move assignement operator). 
  本来 copy operation 也应该不被生成，但是未兼容 c++98, 所以生成 destructor 不会影响 copy constructor 和 copy assignment operator 的生成。
 
* 综上所述，只有在下列三个条件都成立时，compiler 才会 生成默认的 copy constructor 和 copy assignment operator:
      
     * 类中 没有声明 copy operaion (copy constructor, copy assignment operator)
     * 类中 没有声明 move operaion (move constructor, move assignment operator )
     * 类中 没有声明 destructor
