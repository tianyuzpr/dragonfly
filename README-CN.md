# DragonFlyOS - 一个基于Fedora，自主可控且开源的 Linux 发行版！

[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-stage--1%20demo-orange.svg)](docs/10-demo-plan.md)
[![Contributing](https://img.shields.io/badge/contributing-welcome-brightgreen.svg)](CONTRIBUTING.md)
<!-- [![Official-Site](https://img.shields.io/badge/Official-Site-Yes-green.svg)](https://dragonfly.org/) 官网未搭建完成，遂注释，请勿删除  -->

> 🚧 **项目状态：阶段一 demo 已跑通（2026-11-21）**  
> 用 kiwi 构建的 Fedora 45 镜像已能在 QEMU 中启动并到达串口登录提示（内核 `7.2.11-300.fc45.x86_64`）。  
> **目前没有 ISO、没有桌面环境、没有安装器，也不对外分发镜像。**  
> 构建步骤与实测输出见 [docs/10-demo-plan.md](docs/10-demo-plan.md)。

[English](README.md) | 简体中文

## 项目描述
DragonFlyOS 是一个基于 Fedora 的 Linux 发行版，它提供了一个安全，自主可控，开源的环境。它支持最新的硬件和软件，以及丰富的社区支持。DragonFlyOS 的目标是为用户提供一个稳定，安全，方便的 Linux 环境。

## 相比同类项目的优势

- 相比 UOS，DragonFlyOS 的性能和驱动优化更完善。它支持最新的硬件和软件，以及丰富的社区支持。
- 相比 OpenHarmony，DragonFlyOS 依托于 Linux 的完善生态系统，提供了更多的软件支持。
- 相比其他的 Linux 发行版，DragonFlyOS 提供了一个更安全，更可控的环境。

## 构建与运行（阶段一）

当前产物是一个 **2 GiB 的 ext4 文件系统镜像**，没有分区表、没有引导器，所以是直接把内核和
initramfs 交给 QEMU 启动的。目前**没有 ISO，也没有可以写进 U 盘的东西**。

环境要求：Linux 宿主（WSL2 可用）、`podman`、`qemu-system-x86_64`，约 8 GiB 空闲内存。

```bash
git clone https://github.com/tianyuzpr/dragonfly.git
cd dragonfly

# 1. Fedora 45 容器 + kiwi（--privileged 和 /dev 是必需的）
podman run -dit --privileged -v /dev:/dev -v "$PWD:/work" \
  --name dfos-builder registry.fedoraproject.org/fedora:45
podman exec dfos-builder dnf install -y kiwi-cli \
  kiwi-systemdeps-disk-images kiwi-systemdeps-bootloaders

# 2. 构建
podman exec dfos-builder kiwi-ng system build \
  --description /work/kiwi/dfos-initramfs --target-dir /work/out

# 3. 启动（需要 KVM；没有就删掉 -accel kvm）
qemu-system-x86_64 -accel kvm -m 1024 -smp 2 \
  -kernel vmlinuz -initrd initramfs.img \
  -drive file=out/dfos-initramfs-demo.x86_64-0.1.0.ext4,format=raw,if=virtio \
  -append "root=/dev/vda rw console=ttyS0" -nographic
```

demo 镜像内置了一个已知的 root 密码（`dfos`），**仅供串口验证使用**。
完整步骤、四处构建环境的绕坑方法、以及实测数字都在
[docs/10-demo-plan.md](docs/10-demo-plan.md)。

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