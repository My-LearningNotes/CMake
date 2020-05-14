find类指令
==========

* ``find_file`` - This command is used to find a full path to a named file.

    A short-hand signature is:

       ``find_file(<VAR> name1 [path1 path2 ...])``
        
        * ``VAR`` -  存储查找结果的变量;
        * ``name1`` - 要查找的文件的名称;
        * ``path1 path2 ...`` - 指定默认查找路径之外的额外查找路径.

    ``CMAKE_INCLUDE_PATH`` - 用来指定\ ``find_file``\ 和\ ``find_path``\ 的额外查找路径;
    ``CMAKE_SYSTEM_INCLUDE_PATH`` - ``find_file``\ 和\ ``find_path``\ 的系统默认查找路径.

    This command is used to find a full path to a named file. 
    A cache entry named by ``<VAR>`` is created to store the result of this command. 
    If the full path to a file is found the result is stroed in the variable and the search will not be repeated unless the variable is cleared. 
    If nothing is found, the result will be ``<VAR>-NOTFOUND``, and the search will be attemped again the next time find_file is invoked with the same variable.

* ``find_path`` - This command is used to find a directory containing the named file.

    A short-hand signature is:

        ``find_path(VAR name1 [path1 path2 ...])``

   ``find_path``\ 的用法和\ ``find_file``\ 类似, 只不过查找的是包含指定文件的文件夹.

* ``find_library`` - This command is used to find a library.

    A short-hand signature is:
        
        ``find_library(<VAR> name1 [path1 path2 ...])``

    ``CMAKE_LIBRARY_PATH`` - 指定\ ``find_library``\ 的查找路径;
    ``CMAKE_SYSTEM_LIBRARY_PATH`` - ``find_library``\ 的系统默认查找路径.
        
* ``find_program`` - This command is used to find a program.

    A short-hand signature is:

        ``find_program(<VAR> name1 [path1 path2 ...])``

    ``CMAKE_PROGRAM_PATH`` - 额外指定\ ``find_program``\ 的查找路径;
    ``CMAKE_SYSTEM_PROGRAM_PATH`` - ``find_program``\ 的系统默认查找路径.


总结:
    ``find_file``, ``find_path``, ``find_library``\ 和\ ``find_program``\ 的功能用用法类似. 

    对于通常的使用:

        * 第一个参数是一个用来存储查找结果的cache变量, 如果没有找到, 则相应的值为: ``<VAR>-NOTFOUND``;
        * 第二个参数是要查找的目标的名称;
        * 第三个参数是可选的, 用来指定默认查找路径之外的额外查找路径.

    它们的不同之处在于: 它们用来指定查找路径的环境变量不同, 系统默认的查找路径也不同.

