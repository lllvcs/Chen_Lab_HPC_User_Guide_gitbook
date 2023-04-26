# Bioconductor镜像使用帮助

Bioconductor 为高通量基因组数据的分析和可视化提供开源工具。Bioconductor 多数软件包采用 R 统计编程语言开发。Bioconductor 每年释出两个版本，并有活跃的用户社区。

使用方法：Bioconductor 镜像源配置文件之一是`.Rprofile`（linux 下位于 `~/.Rprofile`, Windows 下位于`~\library\base\R\Rprofile`）。

在文末添加如下语句或在R/RStudio终端下键入：

* **校园网：**

**`options(BioC_mirror="http://10.20.252.4/bioconductor")`**

* **HPC集群：**

**`options(BioC_mirror="http://mirrors/bioconductor")`**

打开R即可使用该 Bioconductor 镜像源安装 Bioconductor 软件包。
