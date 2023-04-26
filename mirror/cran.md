# CRAN镜像使用帮助

CRAN (The Comprehensive R Archive Network) 镜像源配置文件之一是`.Rprofile`（linux 下位于`~/.Rprofile`）。

在文件最下方添加如下语句:

* **校园网：** **`options("repos" = c(CRAN="http://10.20.252.4/CRAN/"))`**
* **HPC集群：** **`options("repos" = c(CRAN="http://mirrors/CRAN/"))`**

打开 R 即可使用该 CRAN 镜像源安装 R 软件包。

## Ubuntu 下添加CRAN镜像安装r

**参考如下链接中 README 里的步骤**

```bash
# Debian 帮助
http://10.20.252.4/CRAN/bin/linux/debian/
# Ubuntu 帮助
http://10.20.252.4/CRAN/bin/linux/ubuntu/fullREADME.html
```

例如 `Ubuntu 18.04`

编辑 `sudo vim /etc/apt/sources.list.d/r-tuna.list`

输入 `deb http://10.20.252.4/CRAN/bin/linux/ubuntu bionic-cran40/`

(注意根据你的发行版替换`bionic`，根据需要的版本号替换`cran40`)

然后运行

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo apt-get update
sudo apt-get install r-base-dev
```
