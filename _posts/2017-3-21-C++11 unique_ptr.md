# C++11 unique_ptr #

* 我有一个 unique_ptr member, 我的 getter 不能 return unique_ptr member, 要返回 member.get()

* unique_ptr member 不用初始化，default constructor 将 unique_ptr 初始化成 vide 的, 但本身不为 null. 

## 作为用于工厂函数 factory 的返回类型 ##

[modern effective c++ 中的例子](http://blog.csdn.net/big_yellow_duck/article/details/52317819)

假设我们有 Base class: Investment, 及其子类 Stock(股票), Bond(债券), RealEstate（房地产）
    
    class Investment { ... };

    class Stock : public Investment { ... };

    class Bond : public Investment { ... };

    class RealEstate : public Investment { ... };

工厂函数通常从堆上分配一个对象，然后返回一个指向它的指针，当不再需要它的时候，调用者要负责delete这个对象。那真是完美匹配std::unique_ptr，因为调用者需要为工厂返回的资源负责（即独占资源的所有权），然后std::unique_ptr在它销毁的时候可以自动 delete 它指向的对象。
    
    
     {                  
       ...
       auto pInvestment =                       // pInvestment的类型是
             makeInvestment(*arguments*);       // std::unique_ptr<Investment>
       ...
     }    // 销毁 *pInvestment

deleter 可以是函数对象，也可以是 lambda

    auto delInvmt = [](Investment *pInvestment)   // 自定义的删除器
                {                                 // 一个lambda表达式
                    makeLogEntry(pInvestment); 
                    delete pInvestment;
                };

    template <typename... Ts>
    std::unique_ptr<Investment, decltype(delInvmt)>   // 修改返回类型
    makeInvestment(Ts&&... params)
    {`
        std::unique_ptr<Investment, decltypedelInvmt)>
           pInv(nullptr, delInvmt);                   // 将要返回的指针
        if ( /* a Stock object should be created */ )
        {
            pInv.reset(new Stock(std::forward<Ts>(params)...));
        }
        else if (/* a Bond object should be created */) 
        {
            pInv.reset(new Bond(std::forward<Ts>(params)...));
        }
        else if ( /* a RealEstate obejct should be created */ )
        {
            pInv.reset(new RealEstate(std::forward<Ts>(params)...));
        }

        return pInv;
    }
    
对程序的几点说明:

* 调用者使用 auto 存储 makeInvestment 的返回值，完全不用关心资源的释放。

      auto pInvestment = makeInvestment(*arguments*);
* delInvmt是从makeInvestment返回对象的自定义删除器.所有的自定义删除函数都是接受一个指向需要销毁的对象的原生指针作为参数. 在这个例子中，删除器的行为是调用makelogEntry，然后应用delete.
* 我们使用自定义的删除器的时候，它的类型要作为std::unique_ptr的第二个模板参数。这里 deleter 是 delInvmt, 所以模板参数是 decltype(delInvmt).
   
      std::unique_ptr<Investment, decltype(delInvmt)>
* 解释 

      std::unique_ptr<Investment, decltypedelInvmt)>  pInv(nullptr, delInvmt);      

  makeInvestment的基本策略是先创建一个空的std::unique_ptr，然后指向一个类型合适的对象，然后返回它。为了关联删除器delInvmt和pInvmt，我们把删除器作为第二个构造参数传递给std::unique_ptr。
  
* 用 reset 将 new 出来的对象付给 unique_ptr

      pInv.reset(new Stock(std::forward<Ts>(params)...));
  试图将原生指针（例如new出来的指针）赋值给std::unique_ptr是不会通过编译的，因为这导致一个从原生指针到智能指针的隐式转换，这样的隐式转换是有问题的，所以C++11的智能指针禁止这样的转换。
  
* 对于每个new，我们都用std::forward来完美转发makeInvestment的参数。这样的话，创建对象的构造函数能得到调用者提供的所有的信息。
    
      std::unique_ptr<Investment, decltype(delInvmt)>   // 修改返回类型
      makeInvestment(Ts&&... params)
      {`
          std::unique_ptr<Investment, decltypedelInvmt)>
             pInv(nullptr, delInvmt);                   // 将要返回的指针
          if ( /* a Stock object should be created */ )
          {
              pInv.reset(new Stock(std::forward<Ts>(params)...)); // forward makeInvestment 的参数
          }
          ...
          return pInv;
      }

* 自定义删除器的参数类型是Investment *。不管makeInvestment函数实际创建的对象是什么类型（例如，Stock，Bond，RealEstate）,它最终都会在lambda表达式里作为一个Investment *对象被删除。这意味着我们可以通过基类指针删除派生类对象。为了实现这项工作，基类——Investment——必须要有一个虚析构函数：

       class Investment {
       public:
           ...
           virtual ~Investment();`  // 设计的基本组成成分
           ...
       };
       
* makeInvestment 的返回类型可以写成 auto, 这样更简单和更具有封装性。
    
      template <typename... Ts>
      auto makeInvestment(Ts&&... params)   // C++14
      {
         auto delInvmt = [](Investment* pInvestment) // 自定义删除器
                         {                                                // 内置于makeInvestment
                             makeLogEntry(pInvestment);
                             delete pInvestment;
                         };

          std::unique_ptr<Investment, decltype(delInvmt)> 
            pInv(nullptr, delInvmt); 
                                                  `
          if (...)
          {
              pInv.reset(new Stock(std::forward<Ts>(params)...));
          }
         ...
          return pInv;
      }

* 当你使用默认删除器时（即delete），你有理由假定std::unique_ptr对象的大小和原生指针一样。
* 如果删除器是函数指针，它通常会让std::unique_ptr的大小增加一到两个字(word)。如果删除器是函数对象，std::unique_ptr的大小改变取决于函数对象存储了多少状态。这意味着当自定义删除器可以用 函数 或者 不捕获变量的lambda表达式实现时，lambda实现会更好(返回类型的大小与Investment\* 相同), 带有大状态的函数对象会造成很大的std::unique_ptr. 

      auto delInvmt1 = [](Investment* pInvestment)           // 自定义删除器是
                 {     // 不捕获变量的
                     makeLogEntry(pInvestment);      // lambda表达式
                     delete pInvestment;
                 };

      template <typename... Ts>
      std::unique_ptr<Investment, decltype(delInvmt1)>   // 返回类型的大小
      makeInvestment(Ts&&... args);                       // 与Investment*相同

      void delInvmt2(Investment* pInvestment)    // 自定义删除器是函数
      {
          makeLogEntry(pInvestment);
          delete pInvestment;
      }
                                          `
      template <typename... Ts>                            // 返回类型大小为
      std::unique_ptr<Investment, void (*)(Investment*)>   // Investment* 加上
      makeInvestment(Ts&&... params);                      //  至少一个函数指针的尺寸

* std::unique_ptr有两种形式

    单独的对象形式（std::unique_ptr<T>）, 该形式没有下标引用操作（operator[]）
   
    数组形式（std::unique_ptr<T[]>）， 改形式没有解引用操作（operator*和operator->）
    
* 比起 数组形式（std::unique_ptr<T[]>） , 还是使用 std::array，std::vetcor，std::string     

* 我能想到的（std::unique_ptr<T[]>）唯一有意义场景就是，当你使用C-like风格的API，并且这API返回一个指像数组的指针，数组是从堆分配的，你需要对这个数组负责

* std::unique_ptr 可以简单又高效地转化为std::shared_ptr：

          std::shared_ptr<Investment> sp =    // 把 std::unique_ptr转换为
              makeInvestment(argument);       // std::shared_ptr

这是为什么std::unique_ptr如此适合做工厂函数的关键原因，工厂函数不会知道：独占所有权语义和共享所有权语义哪个更适合调用者。通过返回一个std::unique_ptr，工厂提供给调用者的是最高效的智能指针，但它不妨碍调用者用std::shared_ptr来替换它

### to be remembered ###
* std::unique_ptr是一个智能，快速，只可移动的智能指针，它以独占所有权语义管理资源。
* 默认情况下，通过delete来销毁资源，但可以指定自定义删除器。有状态的删除器和函数指针作为std::unique_ptr的删除器会增加std::unique_ptr对象的大小。
* 将std::unique_ptr转换为std::shared_ptr是容易的。

## unique_ptr 用来实现 Pimpl Idiom ##

参见 effective modernc C++ item 22  
