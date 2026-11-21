# DragonFlyOS - 一个基于Fedora，自主可控且开源的 Linux 发行版！

[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
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
[Apache-2.0](LICENSE)
本项目采用 Apache License 2.0。选择它的理由：
1. 宽松许可：允许使用、修改、再分发与商业使用，包括闭源衍生作品。
2. 第 3 条提供明确的专利授权，对贡献者和使用者都构成保护。
3. 与 GPLv3 兼容：Apache-2.0 代码可以并入 GPLv3 项目（反向不成立）。
4. 要求保留版权与署名声明，但**不要求**衍生作品继续开源。

> 注意：这一点与之前的 GPLv3 选择不同。Apache-2.0 不带 copyleft，下游分叉在保留声明的前提下可以用其他条款分发。

## 贡献指南
详见 [CONTRIBUTING.md](CONTRIBUTING.md)

## 联系我们
- 邮箱: [xiayue1052@outlook.com](mailto:xiayue1052@outlook.com)
- QQ: [1103943279](https://qm.qq.com/q/rwBhWnsQUg)
- QQ加群密码：`dragonflylinux`(大小写敏感)