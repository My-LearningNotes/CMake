``function``\ 和\ ``macro``
===========================

``function``
-------------

::

    function(<name> [arg1 [arg2 [...]]])
        command1(arg ...)
        command2(arg ...)
        ...
    endfunction()

* ``function()``\ 和\ ``endfunction()``\ 必须成对出现, 且他们后面的圆括号必不可少;
* ``function()``\ 和\ ``endfunction()``\ 之间的部分是函数体, 注意缩进;
* ``<name>``\ 是函数名;
* 函数调用时, 可以向函数传递参数;


In addtional to referencing the formal parameters you can reference the **ARGC** variable 
which will be set to the number of arguments passed into the function as well as **ARGV0**,  **ARGV1**,  **ARGV2**, ...
which will have the actual values of the arguments passed in.
    
    * 可以在函数头中定义形参, 在函数体中使用定义的参数;
    * 在函数头中也可以不定义形参, 通过\ ``ARGC``\ 和\ ``ARGV``\ 来引用参数;
        
        * ``ARGC``

            传递给函数的参数个数.

        * ``ARGV``

            传递给函数的参数列表, 各个参数依次用\ ``ARGV0``, ``ARGV1``, ... 表示.

函数调用时, 实参的个数可以比形参多, 但不能比形参少.

    
``macro``
----------

macro的定义方式和function类似.

::

    macro(<name> [arg1 [arg2 [...]]])
        command1(arg ...)
        command2(arg ...)
        ...
    endmacro()


``function`` 和 ``macro`` 的异同
---------------------------------

cmake中的function和macro两者的区别很小, 函数有作用域的概念, 使用范围更广.

* 在function和macro中, 对参数值的引用, 都是以\ ``${arg}``\ 的形式;
* function有作用域的概念, 可以在其中定义局部变量；
* 函数调用时, 将实参传递给形参, 有一个\ **参数传递**\ 的过程;
* 函数中的变量, 在执行过程中动态的去取变量的值;
* macro中, 所有的参数和变量, 都按照\ **宏替换**\ 方式, 在执行之前进行替换操作;
  是一个字符串替换的过程, 这类似C语言预处理器对macro的处理.


来看一个StackOverflow上的例子:

.. code-block:: cmake

    set(var "ABC")

    macro(Moo arg)
        message("arg = ${arg}")  # ${arg}表示引用参数的值
        set(arg "abc")
        message("# After change the value of arg.")
        message("arg = ${arg}")
    endmacro()
    message("=== Call macro ===")
    Moo(${var})


    function(Foo arg)
        message("arg = ${arg}")  # ${arg}表示引用参数的值
        set(arg "abc")
        message("# After change the value of arg.")
        message("arg = ${arg}")
    endfunction()
    message("=== Call function ===")
    Foo(${var})

The output:

    .. image:: images/function_macro.png

**注意:**

    通常, 我们以\ ``${var}``\ 这样的形式表示变量的值;但是, 在\ ``if``\ 语句中, 我们直接用变量名表示变量的值;
    
    但是在\ ``macro``\ 中, 因为其中的参数和变量都是执行宏替换操作, 所以\ ``macro``\ 中的\ ``if``\ 语句, 如果表示变量的值，就需要用\ ``${var}``\ 这样的形式.


一些需要注意的问题
------------------

* 向\ ``function``\ 传递一个列表

    如果要打印一个列表:

    .. code-block:: cmake

        set(arg hello world)

        foreach(var ${arg})
            message(${var})
        endforeach()

    将打印列表的功能写成一个\ ``print_list``\ 函数:

    .. code-block:: cmake

        function(print_list arg)
            foreach(var ${arg})
                message(${var})
            endforeach()
        endfunction()


    定义函数时, 如果函数只有一个参数, 如果想向该函数传递一个列表, 应该将做为实参的列表用双引号包围起来, 否则传递给函数的只是列表的第一个元素.

    .. code-block:: cmake

        set(arg hello world)
        print_list(${arg})

        set(arg hello world)
        print_list("${arg}")  # 用双引号将列表包裹起来

    还有一种方法, 使用\ ``ARGC``\ 和\ ``ARGV``:

    .. code-block:: cmake

        function(print_list)
            message("${ARGC}")
            foreach(var ${ARGV})
                message(${var})
            endforeach()
        endfunction()

* 按引用向函数传递参数(在function中可以修改外部作用域的值)

    - 函数调用时, 传递的是变量的名字\ ``var``, 而不是它的值\ ``${var}``;
    - 在函数内部, 使用\ ``set``\ 设置值的时候, 要加上作用域\ ``PARENT_SCOPE``.


    Example:

    .. code-block:: cmake

        set(var "abc")
    
        function(f1 arg)
            set(${arg} "ABC" PARENT_SCOPE)
        endfunction()

        message("Before calling f1, var = ${var}")
        f1(var)
        message("After calling f1, var = ${var}")

