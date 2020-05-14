使用cmake构建项目
=================


编译项目
--------

* 在构建目录中(通常是\ ``build``\ 目录)中执行\ ``cmake <CMakeLists.txt文件的路径>``, 生成Makefile;

.. code-block:: sh
   :emphasize-lines: 2, 5

    # CMakeLists.txt在当前路径下
    cmake .

    # CMakeLists.txt在当前路径的上一层
    cmake ..

* 生成Makefile后再使用make命令进行编译;
  
  如果需要打印make构建的详细过程, 可以使用 ``make VERBOSE=1`` 或者 ``VERBOSE=1 make`` 命令来构建\

* 如果定义了\ ``install指令``\ , 执行\ ``make install``\ 安装;

* 如果定义了\ *doc*\ 指令, 执行\ ``make doc``\ 生成文档.


清理项目
--------

* 执行 ``make clean`` 来对构建的项目进行清理;
* cmake不支持 ``make distclean``, cmake强烈推荐的是 **外部构建(out-of-source build)**.

