# DragonFlyOS - A Fedora-based, Independent & Open Source Linux Distribution!

[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-FreeSoftware-red.svg)](https://www.fsf.org/)
[![Contributing](https://img.shields.io/badge/contributing-welcome-brightgreen.svg)](CONTRIBUTING.md)
<!-- [![Official-Site](https://img.shields.io/badge/Official-Site-Yes-green.svg)](https://dragonfly.org/) Official site not yet built, keep commented -->

> 🚧 **Project Status: Early Planning / Conceptual Validation Phase**
> DragonFlyOS is currently in the design and technical pre-research phase. **No installable images are available yet**.
> If you are interested in contributing to the design, code contribution, or architecture discussion, please join us!

> ⚠️ This page is translated by AI in Chinese. If you find any errors, please report them in the issue tracker. If you want to get the correct version, please go to the [Chinese README](README-CN.md).

English | [简体中文](README-CN.md)

## Project Description
DragonFlyOS is a Linux distribution based on Fedora, providing a secure, independent, and open source environment. It supports the latest hardware and software, along with a rich community support system. The goal of DragonFlyOS is to offer users a stable, secure, and convenient Linux environment.

## Advantages Over Similar Projects

- Compared to UOS, DragonFlyOS has better performance and driver optimization. It supports the latest hardware and software, as well as extensive community support.
- Compared to OpenHarmony, DragonFlyOS leverages the mature Linux ecosystem, offering broader software compatibility.
- Compared to other Linux distributions, DragonFlyOS provides a more secure and controllable environment.

## Installation Guide
1. Clone the project locally
   ```bash
   git clone https://github.com/dragonflyos/dragonfly.git
   ```
2. Install dependencies
   ```bash
   cd dragonfly
   pip install -r requirements.txt
   ```
3. Run the project
   ```bash
   python ./src/main.py
    or directly
   ./Scripts/build
   ```
4. Burn to USB drive
    - Ensure the USB drive is formatted as FAT32
    - Ensure the USB drive is plugged into the system
    - Ensure the USB device path is correct
    - ⚠️ This operation will format the USB drive, all data will be lost!
   ```bash
   sudo dd if=./dist/dragonfly.iso of=<your-usb-device-path> bs=16M status=progress
   ```

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
