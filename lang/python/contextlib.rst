上下文管理
==========

``with`` 关键字
--------------

紧跟在with后面的语句会被求值，
其返回对象的 ``__enter__`` 方法被调用，这个方法的返回值将被赋值给 ``as`` 关键字后面的变量，
当with后面的代码块全部被执行完之后，又将调用前面返回对象的 ``__exit__`` 方法。

使用 with 关键字，并配合自定义的 ``__enter__`` 和 ``__exit__`` 函数，可以巧妙地实现上下文管理。

contextlib
----------

编写 ``__enter__`` 和 ``__exit__`` 仍然很繁琐，因此Python的标准库contextlib提供了更简单的写法，
比如：

.. code-block:: python3

    from contextlib import contextmanager

    class Query(object):

        def __init__(self, name):
            self.name = name

        def query(self):
            print('Query info about %s...' % self.name)

    @contextmanager
    def create_query(name):
        print('Begin')
        q = Query(name)
        yield q
        print('End')

``@contextmanager`` 这个decorator接受一个generator，用 ``yield`` 语句把 ``with ... as var`` 把变量输出出去，然后，``with`` 语句就可以正常地工作了：

.. code-block:: python3

    with create_query('Bob') as q:
        q.query()
