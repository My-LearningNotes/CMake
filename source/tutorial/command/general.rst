常用指令
========

* ``project <project_name>`` - Set the name of the project.

    Additionally this sets the variables ``<project_name>_SOURCE_DIR`` and ``<project_name>_BINARY_DIR`` to the respective values.

* ``cmake_minimum_required(VERSION major[.minor[.patch]])`` - Set the minimim required version of cmake.

* ``aux_source_directory(<dir> <variable>)`` - 收集指定目录下的所有源文件.

  Collects the names of all source files in the specified directory and stores the list in the ``<variable>`` provided.

* ``add_executable(<name> source1 source2 ... sourceN)`` - Add an executable target.

    Adds an executable target called to be built from the source files listed in the command invocation. 
    The corresponds to the logical target name and must be globally unique within a project.

* ``add_library(<name> [STATIC | SHARED | MODULE] source1 source2 ...... sourceN)`` - Add a library target.

    Add a library target called to be built from the source files listed in the command invovation. 
    The corresponds to the logical target name and must be globally unique within a project.

* ``add_subdirectory(source_dir [binary_dir])`` - Add a subdirectory to the build.

* ``message`` - Display a message to the user.

    ``message([<mode>] "message to display ...")``

    The optional keyword determines the type of message:

        + ``(none)`` = Important information
        + ``STATUS`` = Incidental information(显示时，前缀为\ ``-``)
        + ``WARNING`` = CMake Warning, continue processing
        + ``FATAL_ERROR`` = CMake Error, stop processing and generation

* ``set_target_properties`` - 设置构建目标(executable或library)的属性.

    Targets can have properties that affect how they are built.

    .. code-block:: cmake
      :emphasize-lines: 1, 2, 3, 4
    
      set_target_properties(target1 target2 ...
                            PROPERTIES prop1 value1
                                       prop2 value2
                                       ...)

    Set properties on a target.
    The syntax for the command is to list all the files you want to change, and then provide the values you want to set next.
    You can use any ``prop value pair`` you want and extract it later with the ``get_target_property`` command.

    常用的属性:

        - ``OUTPUT_NAME``
        
          Sets the real name of a target when it is built.

        - ``LINK_FLAGS``

          The ``LINK_FLAGS`` property can be used to add extra flags to the link step of a target.

        - ``COMPILE_FLAGS``

          The ``COMPILE_FLAGS`` property sets addtional compiler flags used to build sources within the target. 
          It may also used to pass addtional preprocessor definitions.

        - ``VERSION`` and ``SOVERSION``

          For shared libraries ``VERSION`` and ``SOVERSION`` can be used to specify the build version and API version respectively. 
          When building or installing appropriate symlinks are created if the platform supports symlinks and the linker support so-names. 
          If only one of both is specified the missing is assumed to have the same version number. 
          For executables ``VERSION`` can be used to specify the build version. 
          When building or installing appropriate symlinks are created if the platform support symlinks.

* ``get_target_property`` - Get a property from a target.

   .. code-block:: cmake
      :emphasize-lines: 1
    
      get_target_property(var target property)

* ``include_directories`` - Add include directories to the build(添加头文件的包含路径).

    .. code-block:: cmake
        :emphasize-lines: 1

        include_directories([AFTER | BEFORE] dir1 dir2 ...)

    By default the directories are appended onto the current list directories. 
    By using ``BEFORE`` or ``AFTER`` you can select between appending or prepending, independent from th default.

* ``link_directories`` - Specify directories in which the linker will look for libraries(添加链接库的路径).

    .. code-block:: cmake
        :emphasize-lines: 1

        link_directories(directory1 directory2 ...)

    The command will apply only to targets created after is is called.

* ``link_libraries`` - Link libraries to all targets added later.

    .. code-block:: cmake
        :emphasize-lines: 1

        link_libraries(lib1 lib2 ...)

* ``target_link_libraries`` - 指定构建目标要链接的库.

    Specify libraries or flags to use when linking a given target and/or its dependencies.

    .. code-block:: cmake
        :emphasize-lines: 1

        target_link_libraries(target lib1 lib2 ...)

    The named <target> must have been created in the current directory by command such as ``add_executable`` or ``add_library``.

* ``add_definitions`` - 为编译器定义宏或变量.

    .. code-block:: cmake
        :emphasize-lines: 1

        add_definitions(-DFOO -DBAR -DVAR1=VALUE1 ...)

* ``add_dependencies`` - Add a dependency between top-level targets.
  
    定义目标的依赖, 确保依赖在目标之前构建.

    .. code-block:: cmake
        :emphasize-lines: 1

        add_dependencies(<target> [<target-dependency>] ...)

    Make a top-level ``<target>`` depend on other top-level targets to ensure that they build before ``target>`` does.
    A top-level target is one created by one of the ``add_execute``, ``add_library``, or ``add_custom_target`` commmands.

* ``set`` - Set a **normal**, **cache** or **environment** variable to a given value.
    
    .. note::

      1. 变量的值设置为空, 表示\ **unset this variable**;
      2. 变量的值不只一个参数, 将这些组成一个list赋给变量.

    * **Set Normal Variable**

        .. code-block:: cmake
            :emphasize-lines: 1

            set(<variable> <value> ...)

        Set the given variable in the current function or directory scope.

    * **Set Cache Entry**

        .. code-block:: cmake
            :emphasize-lines: 1

            set(<variable> <value> ... CACHE <type> <docstring> [FORCE])
            
        定义CACHE变量, 可以跨作用域访问.
            
        The ``type`` must be specified as one of:

            * ``BOOL`` - Boolean ON/OFF value;
            * ``FILEPATH`` - Path to a file on disk;
            * ``PATH`` - Path to a directory on disk;
            * ``STRING`` - A line of text;
            * ``INTERNAL`` - A line of text.

        The ``<docstring>`` must be specified as a line of text providing a quick summary.
        若同名的CACHE变量已经存在, 默认是不覆盖的; 如果设置了\ ``FORCE``\ 选项, 表示覆盖已存在的同名CACHE变量;

    * **Set Envrionment Variable**

        .. code-block:: cmake
            :emphasize-lines: 1

            set(ENV{var} <value> ...)

        设置环境变量, cmake中对环境变量的读写需要用\ ``ENV{}``\ 包裹;

            * 取环境变量的值: ``$ENV{var}``;
            * ``if`` 判断时, 使用 ``ENV{var}``, 不用加 ``$``.

* ``unset`` - Unset a variable, cache variable, or environment variable.

* ``include`` - Load and run CMake code from a file or module(包含其它的cmake脚本).

    .. code-block:: cmake
        :emphasize-lines: 1

        include(<file | module>)

    载入并运行\ ``CMakeLists.txt``\ 文件, 或cmake模块(cmake脚本).

* ``option`` - Provides an option that the user can optionally select.

    .. code-block:: cmake
        :emphasize-lines: 1

        option(<option_variable> "help string describing option" [initial value])

    Provide an option for the user to select as ``ON`` or ``OFF``. 
    If no initial value is provided, ``OFF`` is used.

* ``file`` - File manipulation command.

    支持的操作有:
        
        - **Reading**
        - **Writing**
        - **Filesystem**
        - **Path Conversion**
        - **Transfer**
        - **Locking**

* ``string`` - String operations.

    支持的字符串操作有:

        - **Search and Replace**
        - **Regular Expression**
        - **Manipulation**
        - **Comparision**
        - **Hashing**
        - **Generation**

* ``configure_file`` - Copy a file to another location and modify its contents.

    .. code-block:: cmake
        :emphasize-lines: 1

        configure_file(<input> <output> [COPYONLY] [@ONLY])

    Copies an ``<input>`` file to an ``<output>`` file and substitutes variables referenced as ``@VAR@`` or ``${VAR}`` in the input file content.
    Each variable reference will be replaced with current value of the variable, or the empty string if the variable is not defined.

        * ``COPYONLY``

            Copy the file without replacing any variable references or other content.

        * ``@ONLY``

            Restrict variable replacement to reference of the form ``@VAR@``.

* ``set_property`` - Set a named property in a given scope.

* ``get_property`` - Get a property.

* ``execute_process`` - Execute one or more child processed.

