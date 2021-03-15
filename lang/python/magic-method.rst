魔法方法
========

构造、删除
----------

========================= =======================================================================
 方法名                    功能
========================= =======================================================================
 ``__new__``               申请内存空间，生成类实例。
 ``__init__``              在对象被生成之后调用，分配类属性。
 ``__del__``               对象被销毁时调用。
========================= =======================================================================

访问类属性
---------

========================= =======================================================================
 方法名                    功能
========================= =======================================================================
 ``__getattr__``           当访问该类不存在的属性时，会调用此方法。
 ``__setattr__``           在设置类的属性时，会调用此方法。
 ``__getattribute__``        优先级大于 ``__getattr__``，并且不论被访问的属性是否存在，都会进入此方法。
========================= =======================================================================

序列化相关
----------

========================= =======================================================================
 方法名                    功能
========================= =======================================================================
 ``__getstate__``          当使用 ``deepcopy`` 或者 ``pickle.dump`` 此类时调用，输出需要保存的属性。
 ``__setstate__``          当使用 ``deepcopy`` 或者 ``pickle.load`` 此类时调用，加载需要保存的属性。
 ``__getnewargs__``        保存该函数的返回值，传递给 ``__new__`` 函数。
 ``__getnewargs_ex__``     同上，但是优先级更高。
========================= =======================================================================

类容器
------

.. code-block::

    len()               object.__len__(self)
    self[key]           object.__getitem__(self, key)
    self[key] = value   object.__setitem__(self, key, value)
    del[key]            object.__delitem__(self, key)
    iter()              object.__iter__(self)
    reversed()          object.__reversed__(self)
    in操作              object.__contains__(self, item)
    字典key不存在       object.__missing__(self, key)

一元操作符
----------

.. code-block::

    -           object.__neg__(self)
    +           object.__pos__(self)
    abs()       object.__abs__(self)
    ~           object.__invert__(self)
    complex()   object.__complex__(self)
    int()       object.__int__(self)
    long()      object.__long__(self)
    float()     object.__float__(self)
    oct()       object.__oct__(self)
    hex()       object.__hex__(self)
    round()     object.__round__(self, n)
    floor()     object__floor__(self)
    ceil()      object.__ceil__(self)
    trunc()     object.__trunc__(self)

二元操作符
---------

.. code-block:: 

    +	object.__add__(self, other)
    -	object.__sub__(self, other)
    *	object.__mul__(self, other)
    //	object.__floordiv__(self, other)
    /	object.__div__(self, other)
    %	object.__mod__(self, other)
    **	object.__pow__(self, other[, modulo])
    <<	object.__lshift__(self, other)
    >>	object.__rshift__(self, other)
    &	object.__and__(self, other)
    ^	object.__xor__(self, other)
    |	object.__or__(self, other)

.. code-block::

    +=	object.__iadd__(self, other)
    -=	object.__isub__(self, other)
    *=	object.__imul__(self, other)
    /=	object.__idiv__(self, other)
    //=	object.__ifloordiv__(self, other)
    %=	object.__imod__(self, other)
    **=	object.__ipow__(self, other[, modulo])
    <<=	object.__ilshift__(self, other)
    >>=	object.__irshift__(self, other)
    &=	object.__iand__(self, other)
    ^=	object.__ixor__(self, other)
    |=	object.__ior__(self, other)

.. code-block::

    <	object.__lt__(self, other)
    <=	object.__le__(self, other)
    ==	object.__eq__(self, other)
    !=	object.__ne__(self, other)
    >=	object.__ge__(self, other)
    >	object.__gt__(self, other)
