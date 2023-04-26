# Singularity使用帮助

容器技术是一种以应用软件为中心的虚拟化技术。以应用软件为单元，将软件及所有的依赖打包成容器镜像，打包后的容器镜像可直接拷贝到不同的Linux主机上运行。通过容器技术，可以很好的解决安装软件时，依赖库的安装问题、软件环境的隔离以及软件环境的移植问题。

Singularity为劳伦斯伯克利国家实验室开发专门用于高性能计算场景的容器技术，Singularity完全基于可移植性进行虚拟化，更加轻量级，部署更快，Singularity目前被广泛地各高性能计算中心。

通过Singularity来满足作业运行的软件环境，首先是创建或者获取软件镜像，再将创建好的软件镜像上传到集群上运行；

通过Singularity创建软件镜像，需要在有root权限的Linux主机上，或者在配置好fakeroot的Linux主机上以“fakeroot”的身份进行。

下面将介绍如何在有root权限的主机上安装Singularity，以及如何使用Singularity创建软件镜像；

同时介绍如何在已配置好fakeroot节点上，通过Singularity构建软件镜像。

* 首先介绍在拥有**root**权限的**个人主机**上安装Singularity

```bash
# 安装依赖
yum install -y gcc libuuid-devel squashfs-tools openssl-devel make

# 安装go
export VERSION=1.17.2 OS=linux ARCH=amd64   
wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz 
tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz
rm -f go$VERSION.$OS-$ARCH.tar.gz 
echo 'export PATH=/usr/local/go/bin:$PATH' >> /etc/profile
source /etc/profile

# 安装singularity
export VERSION=3.9.2
wget https://github.com/sylabs/singularity/releases/download/v${VERSION}/singularity-ce-${VERSION}.tar.gz 
tar -xzf singularity-ce-${VERSION}.tar.gz
cd singularity-ce-${VERSION}
./mconfig --prefix=/opt/singularity/${VERSION}
cd builddir/
make && make install
echo "export PATH=/opt/singularity/${VERSION}/bin:\$PATH" >> /etc/profile
```

* 接着介绍如何使用Singularity部署软件镜像，以软件molspin为例

```bash
# 拉取docker镜像创建sandbox格式容器
singularity build --sandbox molspin docker://centos:7.6.1810

# 通过交互的方式进入容器镜像，进行软件的安装
singularity shell -w molspin

# 安装依赖
yum install epel-release centos-release-scl -y
yum install devtoolset-7 armadillo-devel -y
source /opt/rh/devtoolset-7/enable
echo "source /opt/rh/devtoolset-7/enable" >> /environment

# 注册并下载MolSpin安装包, 解压后进入
unzip -d molspin molspin-v1.0.120819.zip
cd molspin && make
cp molspin /usr/local/bin/

# 在安装软件完后退出镜像
exit

# 生成sif文件
singularity build molspin.sif molspin/


# 将创建好的软件镜像上传到高性能计算集群，加载singularity软件环境
# 运行镜像，其中example.msd为输入文件
singularity exec molspin.sif  molspin -p 2 -a example.msd
```

> SIF和sandbox两种格式的镜像是可以相互转换的，可以通过singularity build命令将sandbox格式的镜像转换为SIF格式的镜像，也可以通过singularity build命令将SIF格式的镜像转换为sandbox格式的镜像。
>
> ```bash
> # 1. 将SIF格式的容器转换成sandbox;
> singularity build --sandbox centos76 centos76.sif
>
> # 2. 将sandbox容器镜像转化成SIF格式;
> singularity build centos76.sif centos76
> ```

***

同时，集群上也可以构建Singularity软件镜像，用户可以在节点上使用fakeroot安装软件镜像，具体方式如下

```bash
# 登录节点
ssh <user_name>@10.20.252.1
或
ssh <user_name>@10.20.252.2

# 申请计算节点资源
salloc -N 1 -n 1 -p <partition> -t 1:00:00

# 进入申请到的计算节点(例如c02)
ssh c02

# 拉取docker镜像创建sandbox格式容器
# 创建容器时加入参数--fakeroot
# 在集群上构建软件镜像时，需要注意的是，Singularity容器镜像的创建和运行都需要root权限，因此在创建容器镜像时，需要加入参数--fakeroot，而在运行容器镜像时，也需要加入参数--fakeroot
singularity build --fakeroot --sandbox molspin docker://centos:7.6.1810

# 通过交互的方式进入容器镜像，修改容器镜像
# 加入参数--fakeroot
singularity shell --fakeroot -w molspin

# 安装依赖
yum install epel-release centos-release-scl -y
yum install devtoolset-7 armadillo-devel -y
source /opt/rh/devtoolset-7/enable
echo "source /opt/rh/devtoolset-7/enable" >> /environment

# 注册并下载MolSpin安装包, 解压后进入
unzip -d molspin molspin-v1.0.120819.zip
cd molspin && make
cp molspin /usr/local/bin/

# 在安装软件完后退出镜像
exit

# 生成sif文件
singularity build --fakeroot molspin.sif molspin/

# 运行镜像，其中example.msd为输入文件
singularity exec molspin.sif molspin -p 2 -a example.msd
```

使用fakeroot创建的sandbox镜像，不能直接通过rm -rf 删除，删除方式如下

```bash
# 假设要删除的为文件夹名为molspin的sandbox镜像

# 首先，以可读的模式进入要删除的镜像
singularity shell --fakeroot -w molspin

# 删除掉容器中，基于fakeroot创建的所有文件
rm -rf /* 1>/dev/null 2>&1

# 退出镜像
exit

# 将创建好的软件镜像上传到高性能计算集群，加载singularity软件环境
# 删除掉剩下的
rm -rf molspin
```
