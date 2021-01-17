.. _移动语义和完美转发:

移动语义和完美转发
=================

左值VS右值
----------

C++中所有的值都必然属于左值、右值二者之一。
左值是指表达式结束后依然存在的持久化对象，右值是指表达式结束时就不再存在的临时对象。
所有的具名变量或者对象都是左值，而右值不具名。
很难得到左值和右值的真正定义，但是有一个可以区分左值和右值的便捷方法：看能不能对表达式取地址，如果能，则为左值，否则为右值。

左值引用VS右值引用
-----------------

在C++98中，右值不能被引用。

.. code-block:: cpp

    int a = 10; int& refA = a; // refA是a的别名， 修改refA就是修改a, a是左值，refA是左值引用
    int& b = 1; //编译错误! 1是右值，不能够使用左值引用

C++11中引入了右值引用(rvalue reference)的概念，使用的符号是 ``&&``。

.. code-block:: cpp

    int&& a = 1;  // 实质上就是将不具名(匿名)变量取了个别名
    int b = 1;
    int&& c = b;  // 编译错误！不能将一个左值复制给一个右值引用
    int getTemp()
    {
        return int(0);
    }
    int&& a = getTemp();  // getTemp()的返回值是右值（临时变量）

.. note::

    这里a的类型是右值引用类型(int&&)，但是如果从左值和右值的角度区分它，它实际上是个左值。因为可以对它取地址，而且它还有名字，是一个已经命名的右值。

所以，左值引用只能绑定左值，右值引用只能绑定右值，如果绑定的不对，编译就会失败。

但是，常量左值引用却是个例外，它可以绑定非常量左值、常量左值、右值，而且在绑定右值的时候，常量左值引用还可以像右值引用一样将右值的生命期延长。

.. code-block::

    const int& a = 1;  // 常量左值引用绑定右值，不会报错
 
    int getTemp()
    {
        return int(0);
    }
    const int& a = getTemp();  // 不会报错

移动语义
--------

移动语义狭义上是指移动构造函数和移动赋值函数。

基于“右值是指表达式结束时就不再存在的临时对象”这个特点，可以创建移动构造函数来充分利用该右值（临时变量）的内容，以达到优化性能的目标。

.. code-block:: cpp

    #include <iostream>
    #include <cstring>
    #include <vector>
    using namespace std;
    
    class MyString
    {
    public:
        static size_t CCtor;  // 统计调用拷贝构造函数的次数
        static size_t MCtor;  // 统计调用移动构造函数的次数
    public:
        // 构造函数
        MyString(const char* cstr=0){
            if (cstr) {
                m_data = new char[strlen(cstr)+1];
                strcpy(m_data, cstr);
            }
            else {
                m_data = new char[1];
                *m_data = '\0';
            }
        }
    
        // 拷贝构造函数
        MyString(const MyString& str) {
            CCtor++;
            m_data = new char[ strlen(str.m_data) + 1 ];
            strcpy(m_data, str.m_data);
        }

        // 移动构造函数
        MyString(MyString&& str) noexcept
            :m_data(str.m_data) {  // 直接把参数的资源抢过来，避免复制。
            MCtor++;
            str.m_data = nullptr;  // 不再指向之前的资源
        }

        ~MyString() {
            delete[] m_data;
        }

        char* get_c_str() const { return m_data; }
    private:
        char* m_data;
    };

    size_t MyString::CCtor = 0;
    size_t MyString::MCtor = 0;
    
    int main()
    {
        vector<MyString> vecStr;
        vecStr.reserve(1000);  // 先分配好1000个空间
        for(int i=0;i<1000;i++){
            MyString tmp("hello");
            vecStr.push_back(tmp);  // 调用的是拷贝构造函数
        }
        cout << "CCtor = " << MyString::CCtor << endl;
        cout << "MCtor = " << MyString::MCtor << endl;
        cout << endl;
    
        MyString::CCtor = 0;
        MyString::MCtor = 0;
        vector<MyString> vecStr2;
        vecStr2.reserve(1000);  // 先分配好1000个空间
        for(int i=0;i<1000;i++){
            vecStr2.push_back(MyString("hello");  // 传入右值，调用移动构造函数
        }
        cout << "CCtor = " << MyString::CCtor << endl;
        cout << "MCtor = " << MyString::MCtor << endl;
    }

    /* 运行结果
    CCtor = 1000
    MCtor = 0
    
    CCtor = 0
    MCtor = 1000
    */

从上面的例子可以看到，C++程序在执行时可以分辨参数是左值还是右值，再决定调用哪个构造函数。
而移动构造函数接受一个右值作为参数，可以直接把参数的资源给抢过来，从而避免复制，毕竟临时对象的资源不好好利用也是浪费。

同理，也可以重载 ``=`` 操作符并接受右值引用类型，来构造移动赋值函数。

std::move
-------

基于上面的例子，
对于一个左值，肯定是调用拷贝构造函数，但是有些左值是局部变量，生命周期也很短，能否也调用移动构造函数？

C++11为了解决这个问题，提供了 ``std::move()`` 方法来将左值转换为右值，从而方便应用移动语义。
它其实就是告诉编译器，虽然我是一个左值，但是不要对我用拷贝构造函数，而是用移动构造函数。

.. code-block:: cpp

    for(int i=0;i<1000;i++){
        MyString tmp("hello");
        vecStr2.push_back(std::move(tmp)); // 移动语义，调用的是移动构造函数
    }

通用引用（universal references）
------------------------------

当右值引用和模板结合的时候， ``T&&`` 并不一定表示右值引用，它可能是个左值引用又可能是个右值引用。例如

.. code-block:: cpp

    template<typename T>
    void f( T&& param){
        
    }
    f(10);  // 10是右值
    int x = 10;
    f(x);  // x是左值

.. note::

    在一些复杂情况下，需要通过“引用折叠”规则来判断到底是左值引用还是右值引用。

    - 所有的右值引用叠加到右值引用上仍然使一个右值引用。

    - 所有的其他引用类型之间的叠加都将变成左值引用。

.. warning:: 

    通用引用仅仅发生在 ``T&&`` 下，任何一点附加条件都会使之失效，比如：

.. code-block:: cpp

    template<typename T>
    void f(const T&& param);  // 右值引用

    template<typename T>
    void f(std::vector<T>&& param);  // 右值引用

总结通用引用：传递左值进去，就是左值引用；传递右值进去，就是右值引用。如它的名字，这种类型确实很"通用"，下面要讲的完美转发，就利用了这个特性。

完美转发
--------

所谓转发，就是通过一个函数将参数继续转交给另一个函数进行处理，原参数可能是左值引用类型，可能是右值引用，如果还能继续保持参数的原有特征，那么它就是完美的。

.. code-block:: cpp

    void process(int& i){
        cout << "process(int&):" << i << endl;
    }
    void process(int&& i){
        cout << "process(int&&):" << i << endl;
    }
    
    void myforward(int&& i){
        cout << "myforward(int&&):" << i << endl;
        process(i);
    }
    
    int main()
    {
        int a = 0;
        process(a);  // a是左值 process(int&):0
        process(1);  // 1是右值 process(int&&):1
        process(move(a));  // 移动语义，将a由左值改为右值 process(int&&):0
        myforward(2);
        /*
        右值在函数内部转交给process，然而 ``i `` 虽然是右值引用类型，但其本身是个左值
        process(int&):2
        */
        myforward(move(a));  // 同上，在转发的时候右值变成了左值  process(int&):0
    }

上面的例子就是不完美转发，没有保持调用时期望的右值特性。解决这个问题需要用到c++11提供的 ``std::forward()`` 模板函数：

.. code-block:: cpp

    void myforward(int&& i){
        cout << "myforward(int&&):" << i << endl;
        process(std::forward<int>(i));
    }

    myforward(2);  // process(int&&):2

经过上面的修改可以转发右值，但是由于 ``myforward`` 函数本身不接受左值，这个转发仍然不完美。
要实现真正的完美转发，还需要用到前面提到的通用引用。

.. code-block:: cpp

    #include <iostream>
    #include <cstring>
    #include <vector>
    using namespace std;
    
    void RunCode(int &&m) {
        cout << "rvalue ref" << endl;
    }
    void RunCode(int &m) {
        cout << "lvalue ref" << endl;
    }
    void RunCode(const int &&m) {
        cout << "const rvalue ref" << endl;
    }
    void RunCode(const int &m) {
        cout << "const lvalue ref" << endl;
    }
    
    template<typename T>
    void perfectForward(T && t) {
        RunCode(forward<T> (t));
    }
    
    int main()
    {
        int a = 0;
        int b = 0;
        const int c = 0;
        const int d = 0;
    
        perfectForward(a); // lvalue ref
        perfectForward(move(b)); // rvalue ref
        perfectForward(c); // const lvalue ref
        perfectForward(move(d)); // const rvalue ref
    }

总结
----

1. 有两种值类型，左值和右值。
2. 有三种引用类型，左值引用、右值引用和通用引用。左值引用只能绑定左值，右值引用只能绑定右值，通用引用由初始化时绑定的值的类型确定。
3. 左值和右值是独立于他们的类型的，右值引用类型可能是左值可能是右值。
4. 引用折叠规则：所有的右值引用叠加到右值引用上仍然是一个右值引用；其他引用折叠都为左值引用。
5. 移动语义可以减少无谓的内存拷贝，要想实现移动语义，需要实现移动构造函数和移动赋值函数。
6. ``std::move()`` 将一个左值转换成一个右值，强制使用移动拷贝和赋值函数，这个函数本身并没有对这个左值什么特殊操作。
7. ``std::forward()`` 和通用引用共同实现完美转发。
