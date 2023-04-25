# Module使用帮助

Environment Modules 包是一个简化 shell 初始化的工具，它允许用户在使用 modulefiles 进行会话期间轻松修改其环境。每个模块文件都包含为应用程序配置 shell 所需的信息。模块文件可以由系统上的许多用户共享，并且用户可以拥有自己的集合来补充或替换共享模块文件。

***

## module常见的一些指令

```bash
module help       # 显示帮助信息
module avail      # 显示已经安装的软件环境
module load       # 导入相应的软件环境
module unload     # 删除相应的软件环境
module list       # 列出已经导入的软件环境
module purge      # 清除所有已经导入的软件环境
module switch [mod1] mod2 # 删除mod1并导入mod2
```

***

## 查看可用模块

* `module avail` 或 `ml av`
* 将返回如下提示

```bash
----- /opt/ohpc/pub/moduledeps/gnu9 -----
mpich/3.4.2-ofi    mvapich2/2.3.6    openmpi4/4.1.1 (L)

----- /opt/ohpc/pub/modulefiles -----
cmake/3.21.3    gnu9/9.4.0 (L)    libfabric/1.13.0 (L)    os    prun/2.1 (L)    torch/cpu_2.0    ucx/1.11.2 (L)
conda/23.3_with_py3.10    hwloc/2.5.0 (L)    ohpc (L)    plink/1.90    singularity/3.7.1 (L)    torch/cuda_11.7 (D)

 Where:
  D:  Default Module
  L:  Module is loaded

If the avail list is too long consider trying:

"module --default avail" or "ml -d av" to just list the default modules.
"module overview" or "ml ov" to display the number of modules for each name.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
```

* `(D)`代表缩写默认加载模块, `(L)`代表已加载模块

## 加载模块

* `module load ***(你需要加载的模块)` 或 `ml load ***(你需要加载的模块)`
* 例如加载conda
* `module load conda/23.3_with_py3.10`

## 卸载模块

* `module unload ***(你需要卸载的模块)` 或 `ml unload ***(你需要卸载的模块)`
* 例如卸载conda
* `module unload conda/23.3_with_py3.10`

## 特别说明

* 在首次 `module load conda`加载conda之后
* 请务必运行命令 `conda init bash`
* 来初始化conda环境
* 如需每次使用终端自动加载模块，请在`~/.bashrc`文件中添加对应启用模块命令
* 在 `~/.bashrc`中, `module load conda`模块加载必须早于`conda init`初始化
* conda自定义环境均安装在`~/.conda/`文件夹中
* 在使用`sbatch`提交作业或使用`salloc`进入对应计算节点时，请务必在第一步加载所需要的模块

***

## 编写modulefile来管理自己的软件环境

自己编译安装的软件也可以通过module来进行管理，步骤为：先创建目录用来存放自己的modulefile；然后在创建好的目录下编写modulefile

* 首先，创建目录用来存放自己的modulefile

```bash
mkdir ${HOME}/mymodulefiles   # 创建目录用于放自己的module file
echo "export MODULEPATH=${HOME}/mymodulefiles:\$MODULEPATH" >> ~/.bashrc
source ~/.bashrc # 或者退出重新登录即可
# 编写自己的第一个module file
cd ${HOME}/mymodulefiles
vim myfirstmodulefile
```

* 然后在创建好的目录下编写modulefile，假设在\~/soft/gcc/7.2.0安装了gcc编译器，则可以这么编写modulefile

```bash
#%Module1.0
##
##
module-whatis "my first modulefile"

set topdir "~/soft/gcc/7.2.0"
prepend-path PATH "${topdir}/bin"
prepend-path LIBRARY_PATH "${topdir}/lib"
prepend-path LD_LIBRARY_PATH "${topdir}/lib"
prepend-path LIBRARY_PATH "${topdir}/lib64"
prepend-path LD_LIBRARY_PATH "${topdir}/lib64"
prepend-path CPATH "${topdir}/include"
prepend-path CMAKE_PREFIX_PATH "${topdir}"
setenv CC "${topdir}/bin/gcc"
setenv CXX "${topdir}/bin/g++"
setenv FC "${topdir}/bin/gfortran"
setenv F77 "${topdir}/bin/gfortran"
setenv F90 "${topdir}/bin/gfortran"
```

* 编写好后执行`module avail`即可查看到刚刚写好的modulefile了

下为编写modulefile文件常见的语法

```bash
set           # 设置modulefile内部的变量
setenv        # 设置环境变量
prepend-path  # 效果类似于export PATH=xxx:$PATH
append-path   # 效果类似export PATH=$PATH:xxx
```
