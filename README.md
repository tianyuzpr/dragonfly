# DragonFlyOS - A Fedora-based, Independent & Open Source Linux Distribution!

[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-stage--1%20demo-orange.svg)](docs/10-demo-plan.md)
[![Contributing](https://img.shields.io/badge/contributing-welcome-brightgreen.svg)](CONTRIBUTING.md)
<!-- [![Official-Site](https://img.shields.io/badge/Official-Site-Yes-green.svg)](https://dragonfly.org/) Official site not yet built, keep commented -->

> 🚧 **Project Status: Stage-1 demo boots (2026-11-21)**
> A kiwi-built Fedora 45 image now boots under QEMU and reaches a serial login prompt
> (kernel `7.2.11-300.fc45.x86_64`).
> **There is no ISO, no desktop environment and no installer yet, and no images are distributed.**
> Build procedure and measured output: [docs/10-demo-plan.md](docs/10-demo-plan.md).

> ⚠️ This page is translated by AI in Chinese. If you find any errors, please report them in the issue tracker. If you want to get the correct version, please go to the [Chinese README](README-CN.md).

English | [简体中文](README-CN.md)

## Project Description
DragonFlyOS is a Linux distribution based on Fedora, providing a secure, independent, and open source environment. It supports the latest hardware and software, along with a rich community support system. The goal of DragonFlyOS is to offer users a stable, secure, and convenient Linux environment.

## Advantages Over Similar Projects

- Compared to UOS, DragonFlyOS has better performance and driver optimization. It supports the latest hardware and software, as well as extensive community support.
- Compared to OpenHarmony, DragonFlyOS leverages the mature Linux ecosystem, offering broader software compatibility.
- Compared to other Linux distributions, DragonFlyOS provides a more secure and controllable environment.

## Building and Running (Stage 1)

The current artifact is a **2 GiB ext4 filesystem image** with no partition table and no bootloader,
so it is started by handing the kernel and initramfs to QEMU directly. There is no ISO and nothing to
write to a USB stick yet.

Requirements: a Linux host (WSL2 works), `podman`, `qemu-system-x86_64`, and roughly 8 GiB of free RAM.

```bash
git clone https://github.com/tianyuzpr/dragonfly.git
cd dragonfly

# 1. Fedora 45 container with kiwi (--privileged and /dev are required)
podman run -dit --privileged -v /dev:/dev -v "$PWD:/work" \
  --name dfos-builder registry.fedoraproject.org/fedora:45
podman exec dfos-builder dnf install -y kiwi-cli \
  kiwi-systemdeps-disk-images kiwi-systemdeps-bootloaders

# 2. build
podman exec dfos-builder kiwi-ng system build \
  --description /work/kiwi/dfos-initramfs --target-dir /work/out

# 3. boot (KVM; drop -accel kvm if unavailable)
qemu-system-x86_64 -accel kvm -m 1024 -smp 2 \
  -kernel vmlinuz -initrd initramfs.img \
  -drive file=out/dfos-initramfs-demo.x86_64-0.1.0.ext4,format=raw,if=virtio \
  -append "root=/dev/vda rw console=ttyS0" -nographic
```

The demo image ships a known root password (`dfos`) for serial-console verification only.
Full procedure, the four build-environment workarounds and the measured numbers are in
[docs/10-demo-plan.md](docs/10-demo-plan.md).

## License
[Apache-2.0](LICENSE)

This project is licensed under the Apache License 2.0.

Reasons for choosing this license:
1. Permissive: allows use, modification, redistribution, and commercial use, including in closed-source derivative works.
2. Section 3 grants an explicit patent license from contributors, which protects both contributors and users.
3. Compatible with GPLv3: Apache-2.0 code can be combined into GPLv3 projects (the reverse does not hold).
4. Requires retaining copyright and attribution notices, but does **not** require derivative works to remain open source.

> Note: this differs from the previous GPLv3 choice. Apache-2.0 does not impose copyleft, so downstream forks may be distributed under other terms as long as the notices are kept.

## Contribution Guidelines
See [CONTRIBUTING.md](CONTRIBUTING.md)

## Contact Us
- Email: [xiayue1052@outlook.com](mailto:xiayue1052@outlook.com)
- QQ: [1103943279](https://qm.qq.com/q/rwBhWnsQUg)
- QQ Password：`dragonflylinux`(case sensitive)
