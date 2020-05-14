``find_package``
================


为什要使用\ ``find_package``\ 指令
-----------------------------------

``find_package``\ 的功能和\ ``pkg-config``\ 类似, 用来\ **提取一个项目的元信息**\ , 包括: **头文件包含路径**, **链接参数**, **项目中定义的宏**\ 等.
可以方便我们的使用, 且不用考虑由于导入这个项目而带来的间接依赖.


``find_package``\ 的两种查找模式
--------------------------------

* ``Module``\ 模式

    查找\ ``CMAKE_MODULE_PATH``\ 路径下的\ ``FindXXX.cmake``\ 文件(\ *Find Module*\ 命名为: ``Findxxx.cmake``);
    如果没有找到,再去cmake自带的模块目录\ ``<CMAKE_ROOT>/share/cmake-x.y/Modules/``\ 下去查找.
    通过该\ ``Finddxxx.cmake``\ 来获取该\ *package*\ 的元信息;

* ``Config``\ 模式

    查找\ ``XXX_DIR``\ 路径下的\ ``xxxConfig.cmake``\ 文件;
    如果没有找到, 再去\ ``/usr/local/lib/cmake/xxx/``\ 中查找;
    通过\ ``xxxConfig.cmake``\ 文件获取该\ *package*\ 的元信息.

默认情况下, cmake采用\ **Module模式**\ 查找, 如果没有找到, 再采用\ **Config模式**\ 查找.
 

``find_package``\ 的使用
------------------------

Reduced Signature:

.. code-block:: cmake
    :emphasize-lines: 1, 2

    find_package(<package> [version] [EXACT] [QUIET] [MODULE]
        [REQUIRED] [[COMPONENTS] [components...]])


* ``<package>`` - 要查找的package;

* ``[version]`` - 指定版本号, 格式为: ``major[.minor[.patch]]``;

* ``[EXACT]`` - 精确匹配版本号;

* ``[QUIET]`` - The ``QUIET`` option disables messages if the package cannot be found;

* ``[MODULE]`` -  表示只以\ ``MODULE``\ 的方式查找;

* ``[REQUIRED]`` - 表示该包是必须的, 如果没有找到就停止并退出;

* ``[COMPONENTS]`` - 指出该package依赖的组件.

    如果指定了\ ``REQUIRED``\ 选项, 可以将依赖的组件列在\ ``REQUIRED``\ 后面;


使用\ ``find_package``\ 进行查找后, 可以使用以下变量来判断package是否找到, 以及package的信息:

    * ``<package>_FOUND``

        * ``True`` - Found;
        * ``False`` - Not Found.

    * ``<package>_INCLUDE_DIRS`` or ``<package>_INCLUDES``
        
        头文件的包含路径.

    * ``<package>_LIBRARIES`` or ``<package>_LIBS``
        
        链接参数.

如果找到了头文件包含路径, 可以使用\ ``include_directories``\ 来添加头文件包含路径; 
如果找到了链接参数, 可以使用\ ``target_link_libraries``\ 来添加链接参数.


为了支持各种常见的库和包, CMake自带了很多模块.

* ``cmake --help-module-list`` - 查看CMake支持的模块列表;

    Linux下cmake存放自带模块的路径是: ``/usr/share/cmake/Modules/``;

* ``cmake --help-module <模块文件名>`` - 查看\ *Find Module*\ 中定义的变量.

**Example:**

    * 以\ *bzip2*\ 库为例, 在CMake中有一个\ ``FindBZip2.cmake``\ 模块, 只要使用\ ``find_package(BZip2)``\ 指令进行查找, 
      cmake就会执行查找操作并自动给一些变量赋值以反应查找结果, 之后就可以在cmake脚本中检查查找结果并使用;

    .. code-block:: cmake
        :emphasize-lines: 1, 2, 3, 4, 5, 6, 7, 8
    
        cmake_required_minimum(VERSION 2.8.0)
        project(helloworld)
        add_executable(helloworld hello.c)
        find_package(BZip2)
        if (BZip2_FOUND)
            inlcude_directories(${BZip2_INCLUDES})
            target_link_libraries(helloworld ${BZip2_LIBS})
        endif()


为自己的项目编写Find Modules
----------------------------

采用\ ``Module``\ 模式(虽然\ ``Config``\ 模式支持更强大的功能, 但对多数的应用而言, ``Module``\ 模式足够了);
基本思路是, 在项目中生成一个\ ``Findxxx.cmake``\ 文件, 并在安装时将其安装到适当的位置;

