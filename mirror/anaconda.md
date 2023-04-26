# Anaconda镜像使用帮助

Anaconda 是一个用于科学计算的 Python 发行版，支持 Linux, Mac, Windows, 包含了众多流行的科学计算、数据分析的 Python 包。

**Anaconda 安装包可以到** [**Chen Lab HPC镜像站内**](http://10.20.252.4/anaconda/archive/) **下载**

**在HPC中，Anaconda 安装包可以到`/share/home/share/mirrors/anaconda/archive/`目录内获取**

Chen Lab HPC 还提供了 Anaconda 仓库与第三方源（`conda-forge`、`msys2`、`pytorch`等，查看[完整列表](http://10.20.252.4/anaconda/cloud/)，更多第三方源可以前往校园网联合镜像站查看）的镜像，各系统都可以通过修改用户目录下的`.condarc`文件来使用**Chen Lab HPC 镜像源**。

Windows 用户无法直接创建名为`.condarc`的文件，可先执行`conda config --set show_channel_urls yes`生成该文件之后再修改。

注：由于部分第三方源更新过快难以同步，我们不同步`pytorch-nightly`, `pytorch-nightly-cpu`, `ignite-nightly`这三个包。

* **校园网：**

```bash
channels:
  - defaults
show_channel_urls: true
default_channels:
  - http://10.20.252.4/anaconda/pkgs/main
  - http://10.20.252.4/anaconda/pkgs/r
  - http://10.20.252.4/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://10.20.252.4/anaconda/cloud
  msys2: http://10.20.252.4/anaconda/cloud
  bioconda: http://10.20.252.4/anaconda/cloud
  menpo: http://10.20.252.4/anaconda/cloud
  pytorch: http://10.20.252.4/anaconda/cloud
  pytorch-lts: http://10.20.252.4/anaconda/cloud
  simpleitk: http://10.20.252.4/anaconda/cloud
  nvidia: http://10.20.252.4/anaconda/cloud
```

* **HPC：**

```bash
channels:
  - defaults
show_channel_urls: true
default_channels:
  - http://mirrors/anaconda/pkgs/main
  - http://mirrors/anaconda/pkgs/r
  - http://mirrors/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://mirrors/anaconda/cloud
  msys2: http://mirrors/anaconda/cloud
  bioconda: http://mirrors/anaconda/cloud
  menpo: http://mirrors/anaconda/cloud
  pytorch: http://mirrors/anaconda/cloud
  pytorch-lts: http://mirrors/anaconda/cloud
  simpleitk: http://mirrors/anaconda/cloud
  nvidia: http://mirrors/anaconda/cloud
```

即可添加 Anaconda Python 免费仓库。

运行`conda clean -i`清除索引缓存，保证用的是镜像站提供的索引。

运行`conda create -n myenv numpy`测试一下吧。

## Miniconda 镜像使用帮助

Miniconda 是一个 Anaconda 的轻量级替代，默认只包含了 python 和 conda，但是可以通过 pip 和 conda 来安装所需要的包。

Miniconda 安装包可以到 [http://10.20.252.4/anaconda/miniconda/](http://10.20.252.4/anaconda/miniconda/) 下载。
