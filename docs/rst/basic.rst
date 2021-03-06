标题级别
========

实践表明，只要在文本下面随便敲一种符号并到达一定长度就可以变成标题，而标题的级别可以自动推理得到。


文本样式
========

1. `斜体`

:: 

   `斜体`

2. **粗体**

:: 

   **粗体**

3. ``等宽``

:: 

   ``等宽``

4. 内部链接_

以设置锚点的形式实现，比如：

更多信息查看 内部链接_

这里是其他内容。

.. _内部链接:

这里是被引用的地方，点击上面的更多信息就会跳转到这里。

:: 

   更多信息查看 内部链接_

   这里是其他内容。

   .. _内部链接:

   这里是被引用的地方，点击上面的更多信息就会跳转到这里。

5. `外部链接 <www.baidu.com>`_

:: 

   `外部链接 <www.baidu.com>`_

创建表格
========

1. 简单表格

简单表格不能控制表格参数，且无法合并及拆分单元格。

:: 

   ========== =======
    id         score
   ========== =======
    a          100  
    b          99   
   ========== =======

2. 复杂表格

复杂表格功能全面，但是要使用指令 tabularcolumns_ 来实现。

常用指令
========

- 导入文件 `include`

:: 

   .. include:: file_name.rst

.. warning::

   `include` 指令不可以递归导入

- 代码块 `code-block`

:: 

   .. code-block:: python3

      print("hello rst")

.. _tabularcolumns:

- 创建复杂表格 `tabularcolumns`

:: 

   .. tabularcolumns::

   +-------+-----------------+
   | id    | score           |
   +=======+=====+=====+=====+
   | a     | 100 | 100 | 100 |
   +-------+-----+-----+-----+
   | b     | 99  | 99  | 99  |
   +-------+-----+-----+-----+

.. note::

   使用 `tabularcolumns` 指令时，表格内容不需要缩进

- 公式

行内公式 :math:`A_c = (\pi / 4) d ^ 2`

:: 

    :math:`A_c = (\pi / 4) d^2`

整行公式

.. math:: 

    \alpha{}_t(i) = P(O_1, O_2, ..., O_t, q_t = \lambda{} S_i)

:: 

   .. math:: 

      \alpha{}_t(i) = P(O_1, O_2, ..., O_t, q_t = \lambda{} S_i)

- 图片链接

指令后面接文件相对路径（本地文件）或者网络地址。

:: 

    .. image:: https://github.com/xpmemeda/notes-with-sphinx/raw/master/files/sea.png

效果如下：

.. image:: https://github.com/xpmemeda/notes-with-sphinx/raw/master/files/sea.png

.. warning::

    ``.. image::`` 指令不可以换行。
