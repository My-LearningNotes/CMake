使用\ ``CPack``\ 生成安装包
===========================

``CPack``\ 是\ ``CMake``\ 提供的一个工具, 专门用于打包, 可以生成各种平台上的安装包, 包括二进制安装包和源码安装包.


如何使用
---------

在项目顶层的\ ``CMakeLists.txt``\ 文件尾部添加如下:

    ::

        include(InstallRequiredSystemLibraries)

        # 设置一些CPack相关变量
        ...

        include(CPack)


上面的代码做了以下工作:

    * 导入\ ``InstallRequiredSystemLibraries``\ 模块, 以便之后导入\ ``CPack``\ 模块;
    * 设置\ ``CPack``\ 相关变量;
    * 导入\ ``CPack``\ 模块.

执行打包操作有两种方式:

    * 在构建目录中执行\ ``cpack``\ 指令:

        + 生成二进制安装包: ``cpack -C CPackConfig.cmake``
        + 生成源码安装包: ``cpack -C CPackSourceConfig.cmake``

    * 在构建目录中执行\ ``make package``\ (推荐的方式).


基本设置
--------

通过设置预定义的变量设置\ ``CPack``\ 打包时的参数.

* 设置包类型 - ``set(CPACK_GENERATOR "DEB")``

    指定生成\ ``deb``\ 安装包, 也可以指定生成其它类型的安装包.

* 设置软件包的full version -  ``set(CPACK_PACKAGE_VERSION "1.0.0")``

    ``cpack``\ 还提供了\ ``CPACK_PACKAGE_VERISON_MAJOR``, ``CPACK_PACKAGE_VERSION_MINOR``, ``CPACK_PACKAGE_VERSION_PATCH``.

* 设置软件包名称 - ``set(CPACK_PACKAGE_NAME "xxx")``

    If not specified, it defaults to the project name.

* 设置程序安装后的名称 - ``set(CPACK_DEBIAN_PACKAGE_NAME "xxx")``

* 设置package支持的CPU架构 - ``set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "amd64")``

* 设置priority - ``set(CPACK_DEBIAN_PACKAGE_PRIORITY "Optional")``

* 设置section - ``set(CPACK_DEBIAN_PACKAGE_SECTION "devel")``

* 设置软件包安装位置 - ``set(CPACK_SET_DESTDIR true)``

    Boolean toggle to make CPack use ``DESTDIR`` mechanism when packaging.

    If without ``CPACK_SET_DESTDIR``, CPack uses ``CPACK_PACKAGING_INSTALL_PREFIX`` as a prefix whereas with ``CPACK_SET_DESTDIR`` set, 
    CPack will use ``CMAKE_INSTALL_PREFIX`` as a prefix.

* 设置维护者信息 - ``set(CPACK_DEBIAN_PACKAGE_MAINTATINER "xxx")``

* 设置联系方式 - ``set(CPACK_PACKAGE_CONTACT "xxx")``

* 设置描述信息:

    ``set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "xxx")``

    ``set(CPACK_PACKAGE_DESCRIPTION "xxx")``

* 设置依赖关系 - ``set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6 (>=2.3.1-6), libc6 (<2.4)")``

