智能指针
=======

unique_ptr
----------

``std::unique_ptr`` 不能被多个实例共享的内存管理。也就是说，仅有一个实例拥有内存所有权。

不允许左值赋值，仅允许右值赋值。

.. code-block:: cpp

    // f2 = f1           // 非法，不允许左值赋值
    f2 = std::move(f1);  // 此时f1转移到f2，f1变为nullptr

``std::unique_ptr`` 有以下几个常用的方法：

1. release()：返回该对象所管理的指针，同时释放其所有权；

2. reset()：析构其管理的内存，同时也可以传递进来一个新的指针对象；

3. swap()：交换所管理的对象；

4. get()：返回对象所管理的指针；

5. get_deleter()：返回析构其管理指针的调用函数。

shared_ptr
----------

``std::shared_ptr`` 与 ``std::unique_ptr`` 类似，可以使用 ``make_shared()`` 函数来创建对象。
``std::shared_ptr`` 是使用引用计数的智能指针，可以跟踪引用同一个真实指针对象的智能指针实例的数目，
当最后一个引用对象离开其作用域时，才会释放这块内存。

最好使用 ``make_shared`` 来创建 ``std::shared_ptr``，直接使用裸指针初始化可能会出现bug，
比如下面这段代码：

.. code-block:: cpp

    class A{};

    int main()
    {
        A* a = new A();
        std::shared_ptr<A> ptr1{ a };
        cout << ptr1.use_count() << endl;      // output: 1
        {
            std::shared_ptr<A> ptr2{ a };      // 用同一块内存初始化
            cout << ptr1.use_count() << endl;  // output: 1
            cout << ptr2.use_count() << endl;  // output: 1

        }
        cout << ptr1.use_count() << endl;  // output: 1
        return 0;
    }

ptr1与ptr2虽然是用同一块内存初始化，但是这个共享却并不被两个对象所知道，最终导致程序崩溃。
``std::shared_ptr`` 内部使用两个指针：一个指针用于管理实际的指针；另外一个指针指向一个“控制块”，其中记录了哪些对象共同管理同一个指针。
这是在初始化完成的，所以如果单独初始化两个对象，尽管管理的是同一块内存，它们各自的”控制块“没有互相记录的。
使用std::make_shared就不会出现上面的问题，所以要推荐使用。

weak_ptr
--------

std::shared_ptr可以实现多个对象共享同一块内存，当最后一个对象离开其作用域时，这块内存被释放，但是仍然有可能出现内存无法被释放的情况。
有点像死锁，比如下面这个例子：

.. code-block:: cpp

    struct Node {
        Node(int v) : value(v) {}
        shared_ptr<Node> _pre;
        shared_ptr<Node> _next;
        int value;  
    };  
    void funtest() {
        shared_ptr<Node> sp1(new Node(1));  
        shared_ptr<Node> sp2(new Node(2));  
    
        sp1->_next=sp2;  
        sp2->_pre=sp1;  
    }  
    int main() {
        funtest();  
        return 0;  
    }

根据上面的测试用例，
sp1，sp2，_pre，_next均是shared_ptr的对象，所以上图中两个 Node 的引用计数均为2，
而在出 ``funtest()`` 的作用域时，会对栈空间上的变量进行销毁释放，调用sp1和sp2的析构函数，
然而在 ``std::shared_ptr`` 的析构函数中，只有当引用计数为 1 时，才会释放所指向的空间，
所以在这里sp1和sp2所管理的节点空间是不会被释放的，因此也不会调用~Node()这个析构函数。

``std::weak_ptr`` 可以包含由 ``std::shared_ptr`` 所管理的内存的引用。
但是它仅仅是旁观者，并不是所有者也不会增加引用计数。
但是它可以通过 ``lock()`` 方法返回一个 ``std::shared_ptr`` 对象，从而访问这块内存。

所以只要把上述错误代码 Node 类中的 ``std::shared_ptr`` 改为 ``std::weak_ptr`` 就可以令程序正常工作。
