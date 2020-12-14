从零开始搭建开发环境
===================

创建带有sudo权限的用户
---------------------

.. code-block:: shell

   useradd <user_name> -m -G sudo

.. note::

   -m 表示同时创建用户目录

   -G 指明该用户所在的用户组，而 `sudo` 是系统默认拥有sudo权限的用户组

   更多命令参数可以通过 `useradd -h` 来查看

也可以给一个已存在用户赋予sudo权限：

.. code-block:: shell

   usermod <user_name> -a -G sudo


安装oh-my-zsh
-------------

zsh是一个非常好用的交互式终端，因为其具备众多好用的插件而被广泛使用。

oh-my-zsh则承担了zsh各类插件的管理工作。

首先需要在Linux系统上安装zsh

.. code-block:: shell 

   sudo apt update
   sudo apt install zsh

修改登录时启用的终端类型

.. code-block:: shell

   usermod <user_name> -s /bin/zsh


从github下载安装 `oh-my-zsh <https://github.com/ohmyzsh/ohmyzsh>`_，详见 `README.md`。

一些好用的zsh插件
----------------

- `zsh-autosuggestions <https://github.com/zsh-users/zsh-autosuggestions>`_
- `zsh-syntax-highlighting <https://github.com/zsh-users/zsh-syntax-highlighting>`_
