# DragonFlyOS - A Fedora-based, Independent & Open Source Linux Distribution!

[![License](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-FreeSoftware-red.svg)](https://www.fsf.org/)
[![Contributing](https://img.shields.io/badge/contributing-welcome-brightgreen.svg)](CONTRIBUTING.md)
<!-- [![Official-Site](https://img.shields.io/badge/Official-Site-Yes-green.svg)](https://dragonfly.org/) Official site not yet built, keep commented -->

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
[GPL-v3](LICENSE)

Reasons for choosing this license:
1. GPL-v3 is a free license that allows users to freely use, modify, distribute, and share the software.
2. It is an open source license that allows users to view and modify the software's source code.
3. It is a universal license applicable to all types of software.
4. As a security-focused operating system, this license ensures that derivative works must also remain open source, which allows community code review of derivative versions.

## Contribution Guidelines
See [CONTRIBUTING.md](CONTRIBUTING.md)