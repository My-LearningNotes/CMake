变量简介和常用变量
===================

在一个\ ``CMakeLists.txt``\ 文件中定义的变量, 会传递给其通过\ ``add_subdirectory``\ 或\ ``include``\ 调用的子项目或脚本. 
如果在子项目或脚本中定义了同名的变量, 按照同名覆盖的原则, 局部变量有效.

``cmake``\ 变量有两种定义方式:

    * 在\ ``CMakeLists.txt``\ 或\ ``*.cmake``\ 脚本中, 使用\ ``set``\ 指令定义;
    * 在命令行中, 以\ ``-D``\ 开头定义, 后接\ ``VAR=VALUE``\ 的方式定义.


Variables That Change Behavior
-------------------------------

* ``BUILD_SHARED_LIBS``

    Global flag to cause ``add_library`` to create shared libraries if on.

    If present and true, this will cause all libraries to be built shared unless the library was explicitly added as a static library. 
    This variable is often added to projects as an **Option** so that each user of a project can decide if they want to build the project using shared or static libraries.

* ``CMAKE_BUILD_TYPE``

    Specifies the build type for make based generators.
  
    This specifies what build type will be built in this tree. 
    Possible values are ``empty``, ``Debug``, ``Release``, ``RelWithDebInfo`` and ``MinSizeRel``. 
    This variable is only supported for make based generators.

    .. note::

      常用的构建类型是： ``Debug``/``Release``.

* ``CMAKE_INSTALL_PRRFIX``
  
    Install directory used by install.


Variables That Describe System
-------------------------------

* ``CMAKE_HOST_SYSTEM``

    Name of system CMake is running on.

* ``CMAKE_HOST_SYSTEM_NAME``

    Name of the OS CMake is running on.

* ``CMAKE_HOST_SYSTEM_PROCESSOR``

    The name of the CPU CMake is running on.

* ``CMAKE_HOST_STSTEM_VERSION``

    OS version CMake is running on.

* ``CMAKE_HOST_UNIX``

    True for UNIX and UNIX like operating system.

* ``CMAKE_HOST_WIN32``

    True on windows systems, including win64.

* ``CMAKE_SYSTEM``

    目标机器的系统.

* ``CMAKE_SYSTEM_NAME``

    目标机器的系统名.

* ``CMAKE_SYSTEM_PROCESSOR``

    目标机器的CPU类型.

* ``CMAKE_SYSTEM_VERSION``

    目标系统的版本.

* ``UNIX``

    True 如果目标系统是Unix类Unix-like系统.

* ``Win32``

    True 如果目标系统是Windows系统.


Variables That Control the Build
---------------------------------

* ``CMAKE_ARCHIVE_OUTPUT_DIRECTORY``

    Where to put all ``ARCHIVE`` targets when build.

* ``CMAKE_LIBRARY_OUTPUT_DIRECTORY``

    Where to put all the ``LIBRARY`` targets when build.

* ``CMAKE_RUNTIME_OUTPUT_DIRECTORY``

    Where to put all teh ``RUNTIME`` targets when built.

* ``CMAKE_LIBRARY_PATH_FLAG``

    The flag used to add a library search path to a compiler.
    The flag used to specify a library directory to the compiler.
    On most compilers this is ``-L``.

* ``CMAKE_LINK_LIBRARY_FLAG``

    Flag used to link a library into an executable.
    The flag used to specify a library to link to an executable.
    On most compilers this is ``-l``;

* ``CMAKE_INCLUDE_CURRENT_DIR``

    Automaticallly add the current source and build directories to the include path.
    If this variable is enables, CMake automatically adds in each directory ``${CMAKE_CURRENT_SOURCE_DIR}`` and ``${CMAKE_CURRENT_BINARY_DIR}`` to the include path for this directory. 
    These addtional include directories do not propagate to subdirectories. 
    This is useful mainly for out-of-source builds, where files generated into the build tree are included by files located in the source tree.
    By default ``CMAKE_INCLUDE_CURRENT_DIR`` is **OFF**.


Variables That Provide Information
-----------------------------------

Variables defined by CMake, that give information about the project, and CMake.

* ``CMAKE_PROJECT_NAME`` - The name of the current project.

    .. note::

        指 *top-level project* 的项目名称;

        如果项目中包含 *sub project*, 在 *sub project* 的 *CMakeLists.txt* 中指的也是 *top-level project* 的项目名.


* ``PROJECT_NAME`` - Name of the project given to the project comamnd.

    指当前\ ``CMakeLists.txt``\ 所对应的项目的名称:

        * 如果在\ ``top-level``\  的\ ``CMakeLists.txt``\ 中定义, 指\ ``top-level project``\ 的项目名;
        * 如果在\ ``sub project``\ 的\ ``CMakeLists.txt``\ 中定义, 指\ ``sub project``\ 的项目名.

* ``CMAKE_SOURCE_DIR`` - The path to the top level of the source tree.

    指\ ``top-level project``\ 的源码目录, 即使项目中包含\ ``sub project``\, 在\ ``sub project``\ 的\ ``CMakeLists.txt``\ 中指的也是\ ``top-level project``\ 的源码目录.

* ``PROJECT_SOURCE_DIR`` - Top level source directory for the current project.

    指当前\ ``CMakeLists.txt``\ 所对应的项目的源码目录:

        * 如果在\ ``top-level project``\ 的\ ``CMakeLists.txt``\ 中定义, 指的是\ ``top-level project``\ 的源码目录;
        * 如果在\ ``sub project``\ 的\ ``CMakeLists.txt``\ 中定义, 指的是\ ``sub project``\ 的源码目录.

* ``CMAKE_CURRENT_SOURCE_DIR`` - The path to the source directory currently being processed.

    指cmake当前正在处理的项目的源码目录:

        * 在 *top-level project* 的 *CMakeLists.txt* 中指 *top-level project* 的源码目录;
        * 在 *sub project* 的 *CMakeLists.txt* 中指 *sub project* 的源码目录;

* ``[Project_name]_SOURCE_DIR`` - Top level source directory for the named project.

    A variable is created with the name used in the ``project`` command, and is the source directory for the project. 
    This can be useful when ``add_subdirectory`` is used to connect several projects.

* ``CMAKE_BINARY_DIR`` - The path to the top level of the build tree.

    指顶层项目的构建目录.
    如果顶层项目中包含子项目, 即使在子项目中也是指顶层项目的构建目录.

* ``PROJECT_BINARY_DIR`` - Full path to build directory for project.

    指当前项目的构建目录:

        * 在\ ``top-level project``\ 中, 指\ ``top-level project``\ 的构建目录；
        * 在\ ``sub project``\ 中, 指\ ``sub project``\ 的构建目录.

* ``CMAKE_CURRENT_BINARY_DIR`` - The path to the binary directory currently being processed.

    This is the full path to the build directory that is being process by cmake. 
    Each directory added by ``add_subdirectory`` will create a binary directory in the build tree, and as it is being processed this variable wil be set.

* ``[Project_name]_BINARY_DIR`` - Top level binary directory for the named project.

    A variable is created with the name used int the ``project`` command, and is the binary directory for the project. 
    This can be useful when ``SUBDIR`` is used to connect several projects.

* ``CMAKE_VERBOSE_MAKEFILE`` - Create verbose Makefiles if on.

    This variable defaults to false. 
    You can set this variable to true to make CMake produce verbose Makefiles that show each comamnd line as it is used.

* ``CMAKE_VERSION`` - The full version of cmake in **major.minor.patch** format.

* ``CMAKE_MAJOR_VERSION`` - The major version of cmake.

* ``CMAKE_MINOR_VERSION`` - The minor version of cmake.

* ``CMAKE_PATCH_VERSION`` - The patch version of cmake.

* ``CMAKE_CURRENT_LIST_FILE`` - 表示定义这个变量的 *CMakeLists.txt* 的完整路径.

* ``CMAKE_CURRENT_LIST_LINE`` - 表示定义这个变量所在的行.

