使用源码编译DPDK目标文件
========================

.. note::

    这个过程的部分工作可以通过章节 :ref:`linux_setup_script` 描述的脚本来实现。

安装DPDK及源码
--------------

首先，解压文件并进入到DPDK源文件根目录下：

.. code-block:: console

    tar xJf dpdk-<version>.tar.xz
    cd dpdk-<version>

DPDK源文件由几个目录组成：

*   lib: DPDK 库文件

*   drivers: DPDK 轮询驱动源文件

*   app: DPDK 应用程序 (自动测试)源文件

*   examples: DPDK 应用例程

*   config, buildtools, mk: 框架相关的makefile、脚本及配置文件

DPDK目标环境安装
----------------

DPDK目标文件的格式为::

    ARCH-MACHINE-EXECENV-TOOLCHAIN

其中：

* ``ARCH`` 可以是： ``i686``, ``x86_64``, ``ppc_64``

* ``MACHINE`` 可以是： ``native``, ``power8``

* ``EXECENV`` 可以是： ``linuxapp``,  ``bsdapp``

* ``TOOLCHAIN`` 可以是： ``gcc``,  ``icc``

目标文件取决于运行环境是32位还是64位设备。可以在DPDK的 /config 目录中找到可用的目标，不能使用defconfig\_前缀。

.. note::

    配置文件根据 ``RTE_MACHINE`` 优化级别不同分别提供。在配置文件内部，``RTE_MACHINE`` 配置为 native，
	意味着已编译的软件被调整到其构建的平台上。有关此设置的更多信息，请参阅 *DPDK 编程指南*。

当使用Intel® C++ 编译器 (icc)时，对64位和32位,需要使用以下命令进行调整。
注意，shell脚本会更新 ``$PATH`` 值，因此不能再同一个会话中执行。
此外，还应该检查编译器的安装目录，因为可能不同。

.. code-block:: console

    source /opt/intel/bin/iccvars.sh intel64
    source /opt/intel/bin/iccvars.sh ia32

在顶级目录中使用 ``make install T=<target>`` 来生成目标文件。

例如，为了使用icc编译生成64位目标文件，运行如下命令：

.. code-block:: console

    make install T=x86_64-native-linuxapp-icc

为了使用gcc编译生成32位目标文件，命令如下：

.. code-block:: console

    make install T=i686-native-linuxapp-gcc

如果仅仅只是生成目标文件，并不运行，比如，配置文件改变需要重新编译，使用 ``make config T=<target>`` 命令：

.. code-block:: console

    make config T=x86_64-native-linuxapp-gcc

.. warning::

    任何需要运行的内核模块，如 ``igb_uio``, ``kni``, 必须在与目标文件编译相同的内核下进行编译。
    如果DPDK未在目标设备上构建，则应使用 ``RTE_KERNELDIR`` 环境变量将编译指向要在目标机上使用的内核版本的副本（交叉编译的内核版本）。

创建目标环境之后，用户可以移动到目标环境目录，并继续更改代码并编译。用户还可以通过编辑build目录中的.config文件对DPDK配置进行修改。
(这是顶级目录中defconfig文件的本地副本)。

.. code-block:: console

    cd x86_64-native-linuxapp-gcc
    vi .config
    make

此外，make clean命令可以用于删除任何现有的编译文件，以便后续完整、干净地重新编译代码。

Browsing the Installed DPDK Environment Target
----------------------------------------------

一旦目标文件本创建，它就包含了构建客户应用程序所需的DPDK环境的所有库，包括轮询驱动程序和头文件。
此外，test和testpmd应用程序构建在build/app目录下，可以用于测试。
还有一个kmod目录，存放可能需要加载的内核模块。

加载模块启动DPDK环境需要的UIO功能
---------------------------------

要运行任何的DPDK应用程序，需要将合适的uio模块线加载到当前内核中。在多数情况下，Linux内核包含了标准的 ``uio_pci_generic`` 模块就可以提供uio能力。
该模块可以使用命令加载

.. code-block:: console

    sudo modprobe uio_pci_generic

区别于 ``uio_pci_generic`` ，DPDK提供了一个igb_uio模块（可以在kmod目录下找到）。可以通过如下方式加载：

.. code-block:: console

    sudo modprobe uio
    sudo insmod kmod/igb_uio.ko

.. note::

    对于一下不支持传统中断的设备，例如虚拟功能（VF）设备，必须使用 ``igb_uio`` 来替代 ``uio_pci_generic`` 模块。

由于DPDK 1.7版本提供VFIO支持，所以，对于支持VFIO的平台，可选则UIO，也可以不用。

加载VFIO模块
------------

DPDK程序选择使用VFIO时，需要加载 ``vfio-pci`` 模块：

.. code-block:: console

    sudo modprobe vfio-pci

注意，要使用VFIO，首先，你的平台内核版本必须支持VFIO功能。
Linux内核从3.6.0版本之后就一直包含VFIO模块，通常是默认存在的。不够请查询发行文档以确认是否存在。

此外，要使用VFIO，内核和BIOS都必须支持，并配置为使用IO虚拟化 (如 Intel® VT-d)。

为了保证非特权用户运行DPDK时能够正确操作VFIO，还应设置正确的权限。这可以通过DPDK的配置脚本(dpdk-setup.sh文件位于usertools目录中)。

.. _linux_gsg_binding_kernel:

网络端口绑定/解绑定到内核去顶模块
---------------------------------

从版本1.4开始，DPDK应用程序不再自动解除所有网络端口与原先内核驱动模块的绑定关系。
相反的，DPDK程序在运行前，需要将所要使用的端口绑定到 ``uio_pci_generic``, ``igb_uio`` 或 ``vfio-pci`` 模块上。
任何Linux内核本身控制的端口无法被DPDK PMD驱动所使用。

.. warning::

    默认情况下，DPDK将在启动时不再自动解绑定内核模块与端口的关系。DPDK应用程序使用的任何端口必须与Linux无关，并绑定到 ``uio_pci_generic``, ``igb_uio`` 或 ``vfio-pci`` 模块上。

将端口从Linux内核解绑，然后绑定到 ``uio_pci_generic``, ``igb_uio`` 或 ``vfio-pci`` 模块上供DPDK使用，可以使用脚本dpdk_nic _bind.py（位于usertools目录下）。
这个工具可以用于提供当前系统上网络接口的状态图，绑定或解绑定来自不同内核模块的接口。
以下是脚本如何使用的一些实例。通过使用 ``--help`` or ``--usage`` 选项调用脚本，可以获得脚本的完整描述与帮助信息。
请注意，要将接口绑定到uio或vfio的话，需要先将这两个模块加载到内核，再运行 ``dpdk-devbind.py`` 脚本。

.. warning::

    由于VFIO的工作方式，设备是否可用VFIO是有明确限制的。大部分是由IOMMU组的功能决定的。
	任何的虚拟设备可以独立使用VFIO，但是物理设备则要求将所有端口绑定到VFIO，或者其中一些绑定到VFIO，而其他端口不能绑定到任何其他驱动程序。

    如果你的设备位于PCI-to-PCI桥之后，桥接器将成为设备所在的IOMMU组的一部分。因此，桥接驱动程序也应该从端口解绑定。

.. warning::

    虽然任何用户都可以运行dpdk-devbind.py脚本来查看网络接口的状态，但是绑定和解绑定则需要root权限。

查看系统中所有网络接口的状态：

.. code-block:: console

    ./usertools/dpdk-devbind.py --status

    Network devices using DPDK-compatible driver
    ============================================
    0000:82:00.0 '82599EB 10-GbE NIC' drv=uio_pci_generic unused=ixgbe
    0000:82:00.1 '82599EB 10-GbE NIC' drv=uio_pci_generic unused=ixgbe

    Network devices using kernel driver
    ===================================
    0000:04:00.0 'I350 1-GbE NIC' if=em0  drv=igb unused=uio_pci_generic *Active*
    0000:04:00.1 'I350 1-GbE NIC' if=eth1 drv=igb unused=uio_pci_generic
    0000:04:00.2 'I350 1-GbE NIC' if=eth2 drv=igb unused=uio_pci_generic
    0000:04:00.3 'I350 1-GbE NIC' if=eth3 drv=igb unused=uio_pci_generic

    Other network devices
    =====================
    <none>

绑定设备 ``eth1``,``04:00.1``, 到 ``uio_pci_generic`` 驱动：

.. code-block:: console

    ./usertools/dpdk-devbind.py --bind=uio_pci_generic 04:00.1

或者

.. code-block:: console

    ./usertools/dpdk-devbind.py --bind=uio_pci_generic eth1

恢复设备 ``82:00.0`` 到Linux内核绑定状态：

.. code-block:: console

    ./usertools/dpdk-devbind.py --bind=ixgbe 82:00.0

# Q&As
## Compiling DPDK 18.11 on newer OS kernel version encounter issues:
```bash
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c: In function ‘igbuio_pci_enable_interrupts’:
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:230:6: error: this statement may fall through [-Werror=implicit-fallthrough=]
  230 |   if (pci_alloc_irq_vectors(udev->pdev, 1, 1, PCI_IRQ_MSIX) == 1) {
      |      ^
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:240:2: note: here
  240 |  case RTE_INTR_MODE_MSI:
      |  ^~~~
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:250:6: error: this statement may fall through [-Werror=implicit-fallthrough=]
  250 |   if (pci_alloc_irq_vectors(udev->pdev, 1, 1, PCI_IRQ_MSI) == 1) {
      |      ^
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:259:2: note: here
  259 |  case RTE_INTR_MODE_LEGACY:
      |  ^~~~
In file included from ./include/linux/device.h:15,
                 from /home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:8:
./include/linux/dev_printk.h:158:24: error: this statement may fall through [-Werror=implicit-fallthrough=]
  158 |  dev_printk_index_wrap(_dev_notice, KERN_NOTICE, dev, dev_fmt(fmt), ##__VA_ARGS__)
      |                        ^
./include/linux/dev_printk.h:110:3: note: in definition of macro ‘dev_printk_index_wrap’
  110 |   _p_func(dev, fmt, ##__VA_ARGS__);   \
      |   ^~~~~~~
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:267:3: note: in expansion of macro ‘dev_notice’
  267 |   dev_notice(&udev->pdev->dev, "PCI INTX mask not supported\n");
      |   ^~~~~~~~~~
/home/logan/dpdk1811/build/build/kernel/linux/igb_uio/igb_uio.c:269:2: note: here
  269 |  case RTE_INTR_MODE_NONE:
      |  ^~~~
  CC base/qbman/bman_driver.o
```
This is due to newer version of GCC treats switch/case falls throughs without `break`. We can disable this error message by setting up the `Makefile` ccflags. The `Makefile` is located in `dpdk1811/kernel/linux/igb_uio/Makefile`. Change the content to the following.
```makefile
# SPDX-License-Identifier: BSD-3-Clause
# Copyright(c) 2010-2014 Intel Corporation

include $(RTE_SDK)/mk/rte.vars.mk

#
# module name and path
#
MODULE = igb_uio
MODULE_PATH = drivers/net/igb_uio

#
# CFLAGS
#
MODULE_CFLAGS += -I$(SRCDIR) --param max-inline-insns-single=100
MODULE_CFLAGS += -I$(RTE_OUTPUT)/include
MODULE_CFLAGS += -Winline -Wall -Werror  -Wno-implicit-fallthrough -Wno-error=implicit-fallthrough
MODULE_CFLAGS += -include $(RTE_OUTPUT)/include/rte_config.h

#
# all source are stored in SRCS-y
#
SRCS-y := igb_uio.c

include $(RTE_SDK)/mk/rte.module.mk
```

