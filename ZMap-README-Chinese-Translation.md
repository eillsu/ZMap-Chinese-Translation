ZMap：互联网扫描器
==========================

翻译：eillsu

GitHub：https://github.com/eillsu/ZMap-Chinese-Translation

ZMap是一种快速单包网络扫描器，专为Internet范围的网络调查而设计。在具有千兆以太网连接的典型台式计算机上，ZMap能够在45分钟内扫描整个公共IPv4地址空间。通过10gigE连接和PF_RING，ZMap可以在5分钟内扫描IPv4地址空间。

ZMap在GNU/Linux，Mac OS和BSD上运行。ZMap目前已经完全实现了用于TCP SYN扫描、ICMP、DNS查询、UPnP、BACNET的探测模块，并且可以发送大量UDP探测。如果您正在寻求更多种类的扫描，例如banner抓取或TLS握手，请查看[ZGrab](https://github.com/zmap/zgrab)，这是一个执行有状态应用层握手的ZMap的姊妹项目。

安装
------------

ZMap的最新稳定版本是2.1.1版本，支持Linux，macOS和BSD。它可以通过以下操作系统上的内置包管理器安装：

| OS                                        |                             |
| ----------------------------------------- | --------------------------- |
| Debian and Ubuntu                         | `sudo apt install zmap`     |
| Fedora, CentOS, and RHEL                  | `sudo yum install zmap`     |
| Gentoo                                    | `sudo emerge zmap`          |
| macOS (using [Homebrew](https://brew.sh)) | `brew install zmap`         |
| Arch Linux                                | `sudo pacman -S zmap`       |

可以在INSTALL中找到有关从源代码构建ZMap的说明。

使用
-----

我们在[GitHub Wiki](https://github.com/zmap/zmap/wiki)中提供了使用ZMap的指南。

许可证和版权信息
---------------------

ZMap Copyright 2017 Regents of the University of Michigan

Licensed under the Apache License, Version 2.0 (the "License"); you may not use
this file except in compliance with the License. You may obtain a copy of the
License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See LICENSE for the specific
language governing permissions and limitations under the License.