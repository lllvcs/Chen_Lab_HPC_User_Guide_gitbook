# Conda使用帮助

Conda 是一个开源的软件包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并在它们之间轻松切换，conda支持python、R、 ruby、 lua、 scala、 java、 javaScript、 C/ C++、 fortran等多种语言，还支持大部分生信软件的安装。

首先介绍如何安装conda；接着介绍如何用conda创建虚拟环境和虚拟环境中安装软件；随后简单介绍一个例子，用conda创建python的虚拟环境；随后介绍用conda创建R语言运行环境；接着介绍用conda管理perl语言环境；最后介绍用conda安装生信软件。

***

### 安装conda

```bash
# 获得最新的miniconda安装包；
wget http://mirrors/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh

# 也可以下载anaconda安装包，但是anaconda安装包比较大，而且安装后会自动安装很多的包；
wget http://mirrors/anaconda/archive/Anaconda3-2023.03-Linux-x86_64.sh

# 安装到自己的HOME目录下software/miniconda3中，这个目录在安装前不能存在；
sh Miniconda3-latest-Linux-x86_64.sh -b -p ${HOME}/software/miniconda3

# 安装成功后删除安装包
rm -f Miniconda3-latest-Linux-x86_64.sh

# 将环境变量写入~/.bashrc文件中；
echo "export PATH=${HOME}/software/miniconda3/bin:\$PATH" >> ~/.bashrc

# 或自动初始化conda；
conda init bash

# 退出重新登录或者执行以下命令
source ~/.bashrc

# 检查是否安装成功
conda --version
```

***

### 使用conda创建虚拟环境，并在虚拟环境中安装软件

* 初始化conda

`conda init bash`

* 使用conda创建环境、进入环境、删除环境等

```bash
conda info -e     # 查看已有的conda环境；
source activate env_name        # 进入指定的conda环境；
conda deactivate  # 退出当前的conda环境；
conda create -n env_name [python=<version>] # 创建环境，并指定python版本，或者安装包等；
conda remove -n env_name --all  # 删除指定环境；
```

* 使用conda在虚拟环境中安装软件

```bash
conda list [-n env_name] # 查看当前环境(指定环境)下安装的包；
conda search pkg_name    # 查看安装包；
conda update pkg_name    # 更新指定的安装包；
conda remove pkg_name    # 卸载指定的安装包
conda install pkg_name   # 安装指定的安装包；
```

### 例子：用conda创建python的虚拟环境

创建一个python环境，并在虚拟环境中安装tensorflow gpu 1.14.0版本库

```bash
# 创建时可以指定Python版本，不指定的话会使用默认的版本(miniconda自带的Python版本)；
conda create -n TensorFlow-1.14.0 -y
# 或指定版本
conda create -n TensorFlow-1.14.0 -y python=3.6

# 进入上一步创建好的环境;
source activate TensorFlow-1.14.0 

# 安装相应的库；
conda install tensorflow-gpu=1.14.0 # 根据自己的需要安装相应的库；

# 安装pip并使用pip安装 python 库；
conda install pip     # 环境中需要先安装pip
pip install numpy     # 根据自己的需要安装相应的库

# 退出当前的虚拟环境；
conda deactivate
```

### 例子：用conda创建R语言运行环境

**如果需要用到rstudio，请使用Lico-云工具中的网页版rstudio**

R是用于统计分析、绘图的语言和操作环境。R是属于GNU系统的一个自由、免费、源代码开放的软件，它是一个用于统计计算和统计制图的优秀工具。

**如安装R4.0及以上版本，可使用以下命令：**

```bash
# 创建虚拟环境，并在创建的时候指定安装R 4.1.3版本;
conda create -n myEnvR -c conda-forge r=4.1.3 -y

# 进入上一步创建好的虚拟环境；
source activate myEnvR

# 安装R相关软件，比如biocmanager(bioconductor)
conda install -c conda-forge r-biocmanager

# 安装R语言库，比如stringi
conda install -c conda-forge r-stringi  

# 安装完后就可以用R语言环境了，如果要退出当前环境则执行以下指令；
conda deactivate
```

**如安装R3.6及以下版本，可使用以下命令：**

```bash
# 创建虚拟环境，并在创建的时候指定安装R 3.6.0版本;
conda create -n myEnvR r=3.6.0 -y

# 进入上一步创建好的虚拟环境；
source activate myEnvR

# 安装R语言库，比如stringi
conda install r-stringi  

# 安装完后就可以用R语言环境了，如果要退出当前环境则执行以下指令；
conda deactivate
```

### 例子：用conda管理perl语言环境

```bash
# 创建虚拟环境；
conda create -n myEnv

# 进入创建好的虚拟环境；
source activate myEnv

# 安装perl和cpan
conda install -c bioconda perl perl-app-cpanminus

# 安装perl相关库
cpanm My::Module
```

### 例子：用conda安装生信软件

bioconda 就是一个conda中专门用来提供生信软件的 channel，提供有超过7000个的生信软件，以下是通过conda安装生信软件的例子

```bash
# 创建虚拟环境
conda create -n anvio 
# 切换到创建好的环境
source activate anvio
# 安装anvio
conda install -c bioconda anvio -y
# 安装完后就可以用anvio了，如果要退出当前环境则执行以下指令；
conda deactivate
```

***

### **错误处理**

当遇到如下错误时

**`Solving environment: failed with initial frozen solve. Retrying with flexible solve`**

执行以下指令

```bash
conda update -n base conda
conda update --all
```
