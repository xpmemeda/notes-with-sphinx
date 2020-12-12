设置等宽字体
============

通常来说，大部分字体都是中文和英文分别等宽，但是中英文混合就不等宽了。
中英文不等宽会导致制作 `rst` 表格时感觉非常别扭，因为边界不是正经的直线。

更纱黑体
--------

这是一个字体集合，英文名为Sarasa Gothic，其中的 ``Sarasa Mono SC`` 可以支持中英文混合等宽。

项目地址为 `Sarasa-Gothic <https://github.com/be5invis/Sarasa-Gothic>`_，直接从Releases界面下载名字含有ttf的压缩包，解压后双击其中的 `sarasa-mono-sc-regular.ttf` 文件安装。

安装成功后，操作系统就可以为各类软件提供该字体。

.. warning::

   安装完毕需要重启才能生效

在vscode中设置字体
-----------------

在vscode的 `setting.json` 文件中增加一行：

.. code-block::

   "editor.fontFamily": "Sarasa Mono SC"
