循环语句
========

``while``\ 语句
---------------

::

    while(condition)
        ...
    endwhile()


* ``while()``\ 和\ ``endwhile()``\ 必须成对出现, 且他们后面的圆括号必不可少;
* ``while()``\ 后必须有一个用来做为判断条件的表达式;


``foreach``\ 语句
-----------------

``foreach``\ 指令用来实现迭代操作.

::

    foreach(loop_var 迭代对象)
        ...
    endforeach()

* ``foreach()``\ 和\ ``endforeach()``\ 必须成对出现, 且它们后面的圆括号必不可少;


``foreach``\ 的三种使用方式:

    * 直接列出要迭代的列表或以一个变量的形式给出
      
        ::
        
            foreach(loop_var arg1 arg2 ...)
                ...
            endforeach()

      或

        ::

            # var 是一个列表
            foreach(loop_var ${var})
                ...
            endforeach()

    * 使用\ ``RANGE``\ 来指定迭代的范围

        * ``foreach(loop_var RANGE total)``

            ``RANGE``\ 后只给出一个数值, 表示迭代范围: ``[0, total]``.
        
        * ``foreach(loop_var RANGE start end)``

            表示迭代范围为: ``[start, end]``, 步长为1.

        * ``foreach(loop_var RANGE start end step)``
            
            表示迭代范围从\ ``start``\ 到\ ``end``\ , 步长为\ ``step``.

    * 迭代列表和项的集合

        .. code-block:: cmake
        
            foreach(loop_var IN [LISTS [list1 [...]]] [ITEMS [item1 [...]]])

            # LISTS 后是一组列表
            # ITEMS 后是一组项


        Example:

        .. code-block:: cmake

            set(list1 1 2 3 4 5 6)
            set(list2 a b c d e)
            set(item1 x)
            set(item2 y)
            foreach (loop_var IN LISTS list1 list2 ITEMS item1 item2)
                message(STATUS ${loop_var})
            endforeach()

