生成版本配置文件
=================

根据cmake中设置的版本号, 生成一个版本配置头文件, 这样, 就可以在源代码中使用该头文件, 获取项目版本号.

基本思路:
    * 在\ ``CMakeLists.txt``\ 文件中, 以\ ``VERSION_MAJOR``, ``VERSION_MINOR``\ 和\ ``VERSION_PATCH``\ 定义版本号;

    * 包含一个模板文件:\ ``config.h.in``, 其基本内容如下:

      .. code-block:: text

          #define XXX_VERSION_MAJOR @VERSION_MAJOR@
          #define XXX_VERSION_MINOR @VERSION_MINOR@
          #define XXX_VERSION_PATCH @VERSION_PATCH@

    * 在\ ``CMakeLists.txt``\ 文件中, 使用\ ``configure_file``\ 指令, 以\ ``config.h.in``\ 为模板, 生成\ ``config.h``\ 文件, 
      在生成时, 将\ ``VERSION_MAJOR``, ``VERSION_MINOR``, ``VERSION_PATCH``\ 替换为变量的实际值;

    * 这样, 在源文件中, 就可以包含该头文件, 使用以宏的形式定义的版本号了.

