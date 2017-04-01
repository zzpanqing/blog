# Choose your containers with care #

* 如果希望 insertion 或 erasure 可以被  roll back （Transactional sematics）就必须使用 node-based container.
* 最小化 iteration, pointer, reference 的 invalidation, 应该使用 node-based container, 因为 在 node-based container 上的 insertion, eraseure 不会造成 iterations, pointers, references 的无效。


# Beware the illusion of container-independent code #

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
     
    如果现在不使用 vector 而换成另外一种 container, WCIterator 也是不用变的。
