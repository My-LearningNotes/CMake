``install``
============


安装的需求有两种情况:
    - 代码编译后执行\ ``make install``\ 安装;
    - 打包时指定安装路径.


``CMAKE_INSTALL_PREFIX``
-------------------------

该变量用于指定安装时的路径前缀.

赋值方式:
    * 在\ ``CMakeLists.txt``\ 中使用\ ``set``\ 指令设置该变量的值:

        .. code-block:: cmake

            set(CMAKE_INSTALL_PREFIX <install_path>)
        
        需要注意的是, 该变量要在\ ``project(<project_name>)``\ 指令后面设置.

    * 在执行\ ``cmake``\ 命令时, 在命令行中定义:

          .. code-block:: sh

              cmake -DCMAKE_INSTALL_PREFIX=<install_path>

    * 如果没有在\ ``CMakeLists.txt``\ 中定义\ ``CMAKE_INSTALL_PREFIX``, 也没有在命令行中赋值, 默认值是\ ``/usr/local``.


指定安装路径
-------------

指定安装路径时, 可以使用\ **相对路径**\ 或\ **绝对路径**\ .
使用相对路径时, 是相对 ``CMAKE_INSTALL_PREFIX`` 的相对路径, 如果没有显式设定该变量的值, 默认是 ``/usr/local``.


``install``\ 指令
-----------------

用于定义安装规则, 安装的目标可以包括 **二进制可执行文件, 动态库, 静态库, 普通文件, 目录, 脚本等**.


``install``\ 指令常用选项
^^^^^^^^^^^^^^^^^^^^^^^^^

* ``DESTINATION`` - 指定安装路径.

    + 绝对路径
    + 相对路径(相对\ ``CMAKE_INSTALL_PREFIX``\ 的路径)
    + 安装时重定位

* ``PERMISSEIONS`` - 指定安装文件的权限.

  如果没有显式指定安装文件的权限, 根据文件类型使用默认权限.

Installing Targets
^^^^^^^^^^^^^^^^^^^

* ``TARGETS``\ 定义要安装目标文件, 是通过\ ``add_executable``\ 或\ ``add_library``\ 生成的目标文件, 可以是二进制可执行文件, 动态库或静态库, 目标类型也就相应的有三种(Linux平台):

    + ``ARCHIVE`` - 静态库
    + ``LIBRARY`` - 动态库
    + ``RUNTIME`` - 二进制可执行文件

* 若在一个\ ``install``\ 指令中安装多个目标文件, 先在\ ``TARGETS``\ 后列出安装目标文件列表, 之后需要指定每一个目标文件的类型, 安装路径, 权限等参数.


Example:

    .. code-block:: cmake

        install(TARGETS my_run my lib my_staticlib
            RUNTIME DESTINATION bin
            LIBRARY DESTINATION lib
            ARCHIVE DESTINATION staticlib
        )

Installing Files
^^^^^^^^^^^^^^^^^

安装普通文件, ``install``\ 指令的参数以\ ``FILES``\ 开头, 后接安装文件列表, 多个文件之间用空格分隔;
若安装路径是以相对路径的形式给出, 表示相对当前\ ``source directory``\ 的路径;

.. note::

    安装目标文件时, 若install指令中有多个目标文件, 需要指定每个目标文件的类型, 安装路径等参数;
    但安装普通文件时, 若install指令中有多个普通文件, 将这些普通文件列在FILES后即可, 不需要为每一个普通文件单独指定其参数
    (或者说这些普通文件只能安装到相同的路径下).

Installing Directories
^^^^^^^^^^^^^^^^^^^^^^^

* 安装目录时, 指令以\ ``DIRECTORY``\ 参数开头, 后接要安装的目录列表, 如果有多个要安装的目录, 用空格分隔;

  如果要安装的目录是以相对路径给出, 表示相对当前\ ``source directory``\ 的路径.

* 对于要安装的目录, ``abc``\ 和\ ``abc/``\ 两种写法有很大的区别;

    * 如果目录名不以\ ``/``\ 结尾, 那么这个目录被安装为目标路径下的\ ``abc``; 
    * 如果目录名以\ ``/``\ 结尾, 那么这个目录中的内容将被安装到目标路径下，但不包括这个路径自身.

* 安装目录中的内容时, 还使用使用正则表达式\ **包含/排除**\ 特定文件.


Example:

    .. code-block:: cmake

        install(DIRECTORY icons scripts/
            DESTINATION share/myproj
        )

这条指令的执行结果是:
将icos目录安装到\ ``${CMAKE_INSTALL_PREFIX}/share/myproj``, 将scripts/中的\ **内容**\ 安装到\ ``${CMAKE_INSTALL_PREFIX}/share/proj``, 但不包含scripts目录自身.

