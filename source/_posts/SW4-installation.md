---
title: 地震波传播模拟程序SW4在Linux系统安装过程
date: 2024-04-23 13:28:31
tags:
categories:
 - 整理
 - 地震模拟
---

# 地震波传播模拟程序[SW4](https://geodynamics.org/resources/sw4/about)在Linux系统安装


## 安装流程
---

### 虚拟机安装
1. 下载并安装[VMware Workstation Player](https://www.vmware.com/products/workstation-player.html)。
2. 创建一个新的虚拟机实例 Ubuntu (需要下载并指定[Ubuntu操作系统光盘文件](https://cn.ubuntu.com/download))，硬盘大小设置40g(默认的20g不够)
    ![虚拟机](VMware.png)

### 配置网络
1. VMware虚拟机的网络设置
   ![VMware虚拟机的网络设置](NetSetting.png)
2. VPN允许局域网连接，并查看http和socks端口
    ![VPN](VPNport.png)
1. 通过任务管理器查看VMware使用的IP地址
    ![IP地址](IPv4.png)
2. 根据前面的IP地址和端口号进行Ubuntu虚拟系统代理设置
   ![Ubuntu虚拟系统代理设置](UbuntuNetwork.png)
3. 在Ubuntu虚拟系统中访问谷歌测试是否能上网

### 安装spack ([官方教程](https://spack.readthedocs.io/en/latest/index.html))
1. 安装git。从home位置打开命令行窗口 (ctrl+alt+T)，输入下面的命令 (Ubuntu中命令行窗口复制/粘贴命令快捷键是ctrl+shift+C/V)：
    `sudo apt install git`
2. 把包管理程序spack代码从github中克隆到本地。
    `git clone -c feature.manyFiles=true https://github.com/spack/spack.git`
3. 安装常用包
    `sudo apt update`
    ```
    sudo apt install build-essential ca-certificates coreutils curl environment-modules gfortran git gpg lsb-release python3 python3-distutils python3-venv unzip zip
    ```


### 安装SW4 ([官方教程](https://github.com/geodynamics/sw4/blob/master/doc/SW4-Installation.pdf))
1. 通过包管理程序spack安装SW4 (可以在设置里面搜索power取消锁屏避免下载被中断)
   `. spack/share/spack/setup-env.sh`
   `spack install sw4`
1. 可能会出现一些报错，参考后面可能出现的问题章节处理

### 测试SW4使用
1. 打开命令行载入SW4包 (每次使用SW4都需要这样)
    `. spack/share/spack/setup-env.sh`
    `spack load sw4`
1. 搜索`test_sw4.py`进入所在位置 (注意是编译成功后的位置而不是源代码的位置)，类似下面目录：
    ```
    cd ~/spack/opt/spack/linux-ubuntu22.04-zen3/gcc-11.4.0/sw4-3.0-wv76l5wclueszhmbhvk7wk7dys44jqs2/test/test_sw4.py
    ```
2. 运行测试：
    `./test_sw4.py -d bin`
    其中 '-d bin'表示sw4可执行文件所在的目录，必须在test_sw4.py位置的上一级
3. 命令行显示以下信息时，表示测试通过
    ```
    Running all tests for level 0 ...
    Test # 1 Input file: energy-nomr-2nd-1.in PASSED
    Test # 2 Input file: energy-mr-4th-1.in PASSED
    Test # 3 Input file: energy-mr-sg-order2-1.in PASSED
    Test # 4 Input file: energy-mr-sg-order4-1.in PASSED
    Test # 5 Input file: refine-el-1.in PASSED
    Test # 6 Input file: refine-att-1.in PASSED
    Test # 7 Input file: refine-att-2nd-1.in PASSED
    Test # 8 Input file: tw-att-1.in PASSED
    Test # 9 Input file: tw-att-2.in PASSED
    Test # 10 Input file: tw-topo-att-1.in PASSED
    ...
    ```


## 可能出现的问题
---
* 出现以下类似的报错是由于编译器没有设置成功
    ```...No C compiler...```
    可以手动搜索`~/.spack/<platform>/compilers.yaml` (需要ctrl+H显示隐藏文件才能搜索到)，并按照以下设置编译器：
    ```
    compilers:
    - compiler:
        paths:
            cc: /usr/bin/gcc
            cxx: /usr/bin/g++
            f77: /usr/bin/gfortran
            fc: /usr/bin/gfortran
    ```
* 紧跟`Fetching`后出现以下报错说明是由于网络问题没有下载成功，
    ```Error: ChecksumError: sha256 checksum failed for ...```
    多次运行`spack install sw4`，或者直接安装相应没有下载成功的包即可解决
* 报错: 
    ```Error: KeyError: 'No spec with name h5z-zfp in sw4'```
    把SW4源代码中`~/spack/var/spack/repos/builtin/packages/sw4/package.py`中改成true：
  `variant("zfp", default=True, description="build with ZFP")`
