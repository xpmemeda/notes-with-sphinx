编写shell脚本
============

特殊符号
-------

=============== =======================
 符号            描述
=============== =======================
 $               变量替换
 ${}             变量替换
 ${:}, ${::}     提取字符串
 ${/}, ${//}     字符串模式匹配（替换）
 ${#}, ${##}     字符串模式匹配向左截断
 ${%}, ${%%}     字符串模式匹配向右截断
 ``              命令替换
 $()             命令替换
=============== =======================

.. code-block:: shell

    var = "/dir1/dir2/my.file.txt"
    $var             # /dir1/dir2/my.file.txt
    ${var}           # 同上
    ${var:1:3}       # di
    ${var:1}         # dir/dir/my.file.txt
    ${var:-2}        # xt
    ${var/dir/path}  # /path1/dir2/my.file.txt, 替换第一个
    ${var//dir/path} # /path1/path2/my.file.txt，替换全部
    ${var#*/}        # dir0/dir2/dir3/my.file.txt, 删掉左数第一个'/'及其左边的字符串
    ${var##*/}       # my.file.txt, 删掉左数最后一个'/'及其左边的字符串
    ${var%/*}        # /dir1/dir2/dir3, 删掉右数第一个'/'及其右边的字符串
    ${var%%/*}       # （空值），拿掉右数最后一个'/'及其右边的字符串

.. code-block:: shell

    HOME = $(cd `dirname $0`; pwd) # 本执行文件所在的目录路径保存为HOME

更多内容查看非官方的 `shell特殊字符大全 <https://blog.csdn.net/K346K346/article/details/51819236>`_。
