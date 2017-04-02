# Item 1 : Choose your containers with care #

* 如果希望 insertion 或 erasure 可以被  roll back （Transactional sematics）就必须使用 node-based container.
* 最小化 iteration, pointer, reference 的 invalidation, 应该使用 node-based container, 因为 在 node-based container 上的 insertion, eraseure 不会造成 iterations, pointers, references 的无效。


# Item 2 : Beware the illusion of container-independent code #

* STL 使用了 generalization 的思想：

    * Array 被 generalized 成 container, 其元素类型被参数化
    * function 被 generalized 成 algorithm 其操作的元素也参数化

* sequence (continuous memory) container 提供 push_front, push_back
* node-based conatainer 提供 bidirectional iterator
* 只有vector 可以被 C 函数使用
* 使用的 container 类型改变，不可能不修改代码，为了尽可能修改代码，可使用以下方法:

    * 用 typedefs 定义 container type 和 iterator type, 
    
             // 没有使用  typedefs 的 代码
             class Widget {...};
             <Widget> vw;
             Widget bestWidget;
             ...
             //give bestWidget a value
             vector<Widget>::iterator i =
             // find a Widget with the
             find(vw.begin(), vw.end(), bestWidget); // same value as bestWidget
             
         使用 typedef 后， 
      
              class Widget {...};
              typedef vector<Widget> WidgetContainer;
              typedef WidgetContainer::iterator WCIterator;
              WidgetContainer vw;
              Widget bestWidget;
              ...
              WCIterator i = find(vw.begin(), vw.end(), bestWidget);
              
         如果现在不使用 vector 而换成另外一种 container, WCIterator 也是有效的，不用修改。
        
         typedef 还可以用来简化很长的类型。比如 你有一个 map 是这样的：
      
             map< string,
                  vector<Widget>::iterator, // CIStringCompare is "case-
                  CIStringCompare>          // insensitive string compare;"
                                            //Item 19 describes it
                                            
         你想要使用该 map 的 const_iterator, 你就可以
      
             typedef MyIterator 
               map<string, vector<Widget>::iterator, CIStringCompare>::const_iterator;
               
         就不用每次都写这么 map<...> 一堆.
         
     * 要进一步的 encapsulation 就要使用类，将实现用的 container 放在类中，encapsule container-specific 的信息。比如要生成一个 customer list, 你生成一个 CustomerList class, 将 list 放在 CustomerList 的 private 部分。
     
              class CustomerList {
              private:
                  typedef list<Customer> CustomerContainer;
                  typedef CustomerContainer::iterator CCIterator;
                  CustomerContainer customers;
               public:
               ...// limit the amount of list-specific
                  //information visible through
                };//this interface

# Item 3 : Make copying cheap and correct for objects in containers. #
* copy in copy out, that is the STL way. 每次添加一个元素到 container 中 (insert / push_back), 你添加的是你指定的 object 的一个 copy, 当你从 container 中获取一个元素 (front / back) 你获取的也是 container 中元素的 copy. 所有的元素移动都有 copy 来完成。

    * 产生移动的操作 : 

        * insert / erase 到 vector, string, deque, 
