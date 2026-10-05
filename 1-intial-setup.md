#  Initial Server Setup
This page covers the foundational setup of the operating system and graphical environment.

---

## System requirements 

*   **Operating Systems**: Ubuntu Server 22.04 LTS and Raspberry Pi OS (Bookworm, 64-bit/ARM64) are fully supported.
>⚠︎ WARNING: Avoid Debian Trixie OS on the Raspberry Pi because it lacks the required Java 17 dependencies.
*   **Required Software**: Java 17 (OpenJDK-17-jre) is a strict requirement. 
>⚠︎ WARNING: The TAK Server **will not start** on Java 11 or Java 21. This package is typically installed automatically via package dependencies.
*   **Recommended Hardware**: A Raspberry Pi 4 Model B (4GB RAM minimum) is required for a basic setup. 
>🛈 NOTE: **8GB RAM** is highly recommended if running a desktop environment or multitasking.
*   **Headless (CLI Only)**: A clean install of Ubuntu Server runs with no GUI, using only **200–300 MB of RAM** at idle.
>🛈 NOTE: This is the recommended configuration to maximize hardware resources for your database and active TAK connections.
*   **Lightweight GUI (XFCE)**: If a graphical desktop is strictly required, use **XFCE**.
>🛈 NOTE: It provides a low-bloat desktop environment with minimal overhead, consuming only **350–450 MB of RAM** at idle.

## Install Ubuntu Server 22.04.5 LTS

1. Download the IOS image here: https://releases.ubuntu.com/22.04.5/ubuntu-22.04.5-live-server-amd64.iso
2. Create a bootable USB flash drive with an image writer (Raspberry Pi Imager)
>🛈 NOTE: When selecting an operating system (OS) choose **Use custom**, and select the downloaded IOS image. 
3. Boot the newly created image from Raspberry Pi imager from the USB flash drive to the desired computer

## Update system package definition and apply security patches

```
sudo apt update && sudo apt upgrade -y
```

## Install core Xfce and LightDM (Display Manager)

```
sudo apt install --no-install-recommends xorg lightdm slick-greeter xfce4 -y
```

Set the Raspberry Pi to boot directly into the GUI:

```
sudo systemctl set-default graphical.target
```

Restart to launch straight into the lightweight login screen:

```
sudo reboot
```

## Install Xarchiver (the standard Xfce compression tool)

```
sudo apt install xarchiver
```

## Install Firefox

```
sudo snap install firefox (or sudo apt install firefox)
```
