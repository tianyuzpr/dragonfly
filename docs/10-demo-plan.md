# 10 · 第一个可启动 demo（阶段一）

> 状态：**已跑通（2026-11-21）**。产物在 WSL 内，未入库、未对外分发。
> 范围：只到"内核 + initramfs + 根文件系统能起来"，不含桌面、安装器、签名与公告。

## 0. 结论

| 项 | 结果 |
|---|---|
| 是否可启动 | **是**。QEMU 冷启动到 `fedora login:`，root 登录成功 |
| 内核 | `7.2.11-300.fc45.x86_64`（镜像内 `uname -r` 实测一致） |
| 发行版标识 | `Fedora release 45 (Forty Five)` |
| 根文件系统 | `/dev/vda on / type ext4 (rw,relatime)` |
| 构建产物 | `dfos-initramfs-demo.x86_64-0.1.0.ext4`，2.0 GiB |
| 构建总耗时 | **约 35 秒**（装包 ~25 s + 生成镜像 ~10 s） |
| 构建宿主 | WSL2 Ubuntu 26.04 + podman 5.7.0 + Fedora 45 容器 |

**已知缺口**：没有分区磁盘镜像，也没有引导器。当前用 QEMU `-kernel` / `-initrd` 直接喂内核启动，`grub` 一段尚未验证（原因见 §4）。

## 1. 构建宿主（实测）

| 项 | 值 |
|---|---|
| CPU | i5-1135G7，4 核 8 线程 |
| 内存 | 15.8 GB 总量；WSL 分配 7.8 GB + 2 GB swap |
| 磁盘 | KIOXIA NVMe 1 TB，WSL 可用 954 GB |
| WSL | 2.7.11，内核 6.18.33.2-2 |
| 网络 | 单连接约 738 Mbps（TUNA 镜像，10 秒采样） |
| 构建容器 | `registry.fedoraproject.org/fedora:45`（199 MB） |
| kiwi | 11.1.16（`dnf install kiwi-cli kiwi-systemdeps-disk-images kiwi-systemdeps-bootloaders`） |
| 容器权限 | **必须 `--privileged` 且 `-v /dev:/dev`**，否则 kiwi 无法挂载、无法建环回设备 |

## 2. 实际走通的流程

### 2.1 准备构建容器

```bash
podman pull registry.fedoraproject.org/fedora:45
podman run -dit --privileged -v /dev:/dev -v /root/dfos-build:/build \
  --name dfos-builder-root registry.fedoraproject.org/fedora:45
podman exec dfos-builder-root dnf install -y kiwi-cli \
  kiwi-systemdeps-disk-images kiwi-systemdeps-bootloaders
```

### 2.2 三处必须改的构建环境（不改必然失败）

| 问题 | 现象 | 处理 |
|---|---|---|
| Fedora 45 已无 dnf4 | `chroot: dnf-3: No such file or directory` | `config.xml` 写 `<packagemanager>dnf5</packagemanager>` |
| `%_pkgverify_level all` | `Error: GPG check FAILED`，repo 级 `gpgcheck=0` 无效 | 容器内把 `/usr/lib/rpm/macros` 的 `%_pkgverify_level` 改为 `digest` |
| 全局 gpgcheck | 同上 | 容器 `/etc/dnf/dnf.conf` 加 `gpgcheck=0` |

> 这三条都是**构建期**行为。要恢复"校验链完整"，正解是导入 Fedora 公钥并撤回以上三处，而不是长期关着。

### 2.3 构建与启动

```bash
kiwi-ng system build --description /build/dfos-initramfs --target-dir /build/out
# 产物：dfos-initramfs-demo.x86_64-0.1.0.ext4

qemu-system-x86_64 -accel kvm -m 1024 -smp 2 \
  -kernel vmlinuz -initrd initramfs.img \
  -drive file=root.ext4,format=raw,if=virtio \
  -append "root=/dev/vda rw console=ttyS0" -nographic
```

### 2.4 验收实测输出

```text
Fedora Linux 45 (Forty Five)
Kernel 7.2.11-300.fc45.x86_64 on x86_64 (ttyS0)
fedora login: root
[root@fedora ~]# uname -r
7.2.11-300.fc45.x86_64
[root@fedora ~]# cat /etc/fedora-release
Fedora release 45 (Forty Five)
[root@fedora ~]# mount | grep " / "
/dev/vda on / type ext4 (rw,relatime)
[root@fedora ~]# echo DFOS_BOOT_OK
DFOS_BOOT_OK
```

## 3. 镜像类型：为什么不是 `disk`

kiwi 11 的 `<type image="...">` **不接受 `disk`**。合法值：
`btrfs | cpio | docker | ext2 | ext3 | ext4 | iso | oem | pxe | kis | squashfs | erofs | tbz | xfs | oci | appx | enclave | wsl`。

本次用 `image="ext4"`（无分区表的纯文件系统镜像），因为它能绕开分区挂载问题，且足够验证"内核能起来"。

## 4. 已知缺口（下一步的输入）

| 缺口 | 现象 | 原因 | 出路 |
|---|---|---|---|
| 分区磁盘镜像 | `partx: /dev/loop0: error adding partition 1` | 容器内无 udev，内核不生成 `/dev/loop0p1` 设备节点 | 换真机/带 udev 的 Fedora 构建，或用 `kiwi-ng boxbuild` |
| 引导器 | grub 未验证 | 上一条的直接后果 | 同上 |
| 桌面 | 未构建 | 本阶段不做 | 阶段二 |

## 5. demo 的默认设置（按你的决定）

| 项 | 值 | 说明 |
|---|---|---|
| 包签名校验 | **构建期关闭** | 见 §2.2，仅限本机构建 |
| root 密码 | `dfos` | **仅供串口验证**，写在 `config.sh` |
| 时区 | `Asia/Shanghai` | 需 `tzdata`，否则 kiwi 报时区无效 |
| 未做 | 开发者模式开关、安装提示 UI | 按你说的"到时候再说" |

## 6. 文件位置

| 内容 | 路径 |
|---|---|
| kiwi 描述 | `kiwi/dfos-initramfs/config.xml`、`config.sh`（已入库） |
| 构建产物 | WSL `/root/dfos-build/out/`（**未入库**） |
| 引导用的内核 / initramfs | WSL `/root/dfos-build/{vmlinuz,initramfs.img}` |

## 7. 实测记录

| 项 | 预期 | 实测 |
|---|---|---|
| 构建耗时 | — | **约 35 s** |
| 镜像大小 | — | **2.0 GiB**（`<size>` 固定 2048 MB） |
| initramfs 大小 | — | 25 MiB |
| 内核大小 | — | 19 MiB |
| 启动到 login 提示 | — | **约 8 s**（KVM，1 GB 内存） |
| 峰值内存 | — | **未测**（构建时未采样） |

## 8. 压缩与体积的取舍（记录理由，便于复算）

- **不用 xz**：4 核下 xz 压 6 GB 约 40–90 分钟，zstd -1 只要 1–2 分钟；磁盘不是约束（可用 954 GB）。
- **不拿用户下载速度当依据**：千兆链路下 6 GB 十几秒就下完，压缩率省下的差异只有几十秒。
- **不主动做大**：不必为省一两 GB 牺牲构建时间，也不主动把镜像做得比同样功能的镜像更大。两句都成立，不互相抵消。

## 9. 下一步

1. 补引导器：在带 udev 的环境出 `oem` 分区镜像，验证 grub 能起（当前唯一硬缺口）。
2. 阶段二：加 KDE 与中文字体 / 输入法，量体积与构建时长。
3. 恢复签名校验：导入 Fedora 公钥，撤回 §2.2 的三处改动。
