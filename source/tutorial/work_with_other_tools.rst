cmake和其他工具的配合使用
=========================

* ``cmake`` 和 ``pkg-config``

  + 使用cmake构建时自动生成项目的pkg config file;
  + ``make install`` 安装时, 将pkg config file安装到适当的路径下;
  + 项目构建完成后打包, 在安装package时, 将pkg config file安装到适当的路径下.

* ``cmake`` 和 ``doxygen``

  + 添加 ``doc`` 指令, 执行 ``make doc`` 时调用 ``doxgen`` 自动生成文档.
  
