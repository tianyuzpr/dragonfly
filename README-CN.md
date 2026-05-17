# DragonFlyOS - 一个基于Fedora，自主可控且开源的 Linux 发行版！

[![License](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-FreeSoftware-red.svg)](https://www.fsf.org/)
[![Contributing](https://img.shields.io/badge/contributing-welcome-brightgreen.svg)](CONTRIBUTING.md)
<!-- [![Official-Site](https://img.shields.io/badge/Official-Site-Yes-green.svg)](https://dragonfly.org/) 官网未搭建完成，遂注释，请勿删除  -->

> 🚧 **项目状态：早期规划 / 概念验证阶段**  
> DragonFlyOS 目前处于设计和技术预研阶段，**尚未发布可安装的镜像**。  
> 如果你对参与设计、贡献代码或讨论架构感兴趣，欢迎加入我们！

[English](README.md) | 简体中文

## 项目描述
DragonFlyOS 是一个基于 Fedora 的 Linux 发行版，它提供了一个安全，自主可控，开源的环境。它支持最新的硬件和软件，以及丰富的社区支持。DragonFlyOS 的目标是为用户提供一个稳定，安全，方便的 Linux 环境。

## 相比同类项目的优势

- 相比 UOS，DragonFlyOS 的性能和驱动优化更完善。它支持最新的硬件和软件，以及丰富的社区支持。
- 相比 OpenHarmony，DragonFlyOS 依托于 Linux 的完善生态系统，提供了更多的软件支持。
- 相比其他的 Linux 发行版，DragonFlyOS 提供了一个更安全，更可控的环境。

## 安装说明
1. clone 项目到本地
   ```bash
   git clone https://github.com/dragonflyos/dragonfly.git
   ```
2. 安装依赖
   ```bash
   cd dragonfly
   pip install -r requirements.txt
   ```
3. 运行项目
   ```bash
   python ./src/main.py
   或直接
   ./Scripts/build
   ```
4. 烧录到 U 盘
    - 确保 U 盘已格式化为 FAT32 格式
    - 确保 U 盘已插入载入系统
    - 确保 U 盘设备路径正确
    - ⚠️ 此操作会格式化 U 盘，所有数据将丢失！
   ```bash
   sudo dd if=./dist/dragonfly.iso of=<你的U盘设备路径> bs=16M status=progress
   ```

## 许可证
[GPL-v3](LICENSE)
我们选择本许可证的理由：
1. GPL-v3 是一个自由的许可证，它允许用户自由地使用，修改，分发和修改软件。
2. 它是一个开源的许可证，它允许用户查看和修改软件的源代码。
3. 它是一个通用的许可证，它适用于所有类型的软件。
4. 作为一个安全性较高的操作系统，该许可证确保衍生作品也必须开源，这可以确保衍生版的代码也可被社区审查。

## 贡献指南
详见 [CONTRIBUTING.md](CONTRIBUTING.md)

## 联系我们
- 邮箱: [xiayue1052@outlook.com](mailto:xiayue1052@outlook.com)
- QQ: [1103943279](https://qm.qq.com/q/rwBhWnsQUg)
- QQ加群密码：`dragonflylinux`(大小写敏感)