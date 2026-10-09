# Step 1: Initial Server Setup

This guide covers the foundational setup of your operating system, updating package definitions, configuring a lightweight graphical user interface (GUI), and installing essential desktop tools.

---

## 1.1 System Requirements & Baseline Performance

To ensure a stable TAK Server deployment, verify your environment matches the specifications below.

| Component | Minimum Specification | Recommended Specification |
|:---|:---|:---|
| **Operating System** | Ubuntu Server 22.04 LTS (64-bit) | Ubuntu Server 22.04 LTS (64-bit) |
| **Hardware (Pi)** | Raspberry Pi 4 Model B (4GB RAM) | Raspberry Pi 5 (8GB RAM) |
| **Java Runtime** | OpenJDK 17 Runtime Environment | OpenJDK 17 Runtime Environment |

> [!WARNING]
> **Operating System Compatibility:** Avoid installing Debian Trixie OS on a Raspberry Pi. It lacks the required stable Java 17 dependencies.
> <br>
> <br>**Java Version Compatibility:** TAK Server **requires Java 17 (OpenJDK-17)**. It will fail to initialize if run on Java 11 or Java 21.

## 1.2 RAM Utilization Baselines
Choose your desktop environment configuration based on your hardware constraints:
*   **Headless (CLI Only):** Recommended. Uses **200–300 MB RAM** at idle, reserving maximum system resources for your database and active client connections.
*   **Lightweight GUI (XFCE):** Uses **350–450 MB RAM** at idle. Provides a graphical desktop with minimal system overhead.

---

## 1.3 Operating System Installation

Select the installation path matching your target hardware:

### Path A: Raspberry Pi Hardware
1. Launch the **Raspberry Pi Imager** on your workstation.
2. Click **CHOOSE OS** > **Raspberry Pi OS (Other)** > **Raspberry Pi OS Lite (64-bit)** (Bookworm).
3. Select your storage drive and click **WRITE**. 

### Path B: Dedicated PC / x86 Hardware (CLI Only)
1. Download the official installation image: [Ubuntu Server 22.04.5 LTS ISO](https://releases.ubuntu.com/22.04.5/ubuntu-22.04.5-live-server-amd64.iso).
2. Burn the ISO to a USB flash drive using **Raspberry Pi Imager** (Select **Use Custom** for the OS and point to your downloaded ISO file).
3. Boot your target computer from the USB drive and follow the on-screen prompts to complete the OS installation.

---

## 1.4 System Updates & Dependencies

Once your operating system is installed and you are logged into the command-line interface, fetch the latest package definitions and apply security patches:

```bash
sudo apt update && sudo apt upgrade -y
```
---

## 1.5 Lightweight Desktop Environment (Optional)
If a graphical user interface is strictly required for your deployment, follow these steps to install XFCE and LightDM.

1. Install Xorg, LightDM, and the XFCE Desktop:
   ```bash
   sudo apt install --no-install-recommends xorg lightdm slick-greeter xfce4 -y
   ```

2. Configure the System to Boot Directly to the GUI:
   ```bash
   sudo systemctl set-default graphical.target
   ```

3. Reboot the System to Apply Changes:
   ```bash
   sudo reboot
   ```
   *Your server will restart and load into the lightweight LightDM login screen.*

---

## 1.6 Essential Desktop Utilities
Once logged into your XFCE desktop, open a terminal window and install the following baseline tools:

1. File Compression Utility (Xarchiver)
   <br>Install the standard XFCE compression tool to unpack TAK Server packages:
   ```bash
   sudo apt install xarchiver -y
   ```

2. Web Browser (Firefox)
   <br>Install Firefox to easily access your local TAK Server Web Admin dashboard:
   ```bash
   sudo apt install firefox -y
   ```
   *(For default Ubuntu Server installations utilizing Snap packages, use `sudo snap install firefox` instead).*
   
---

### Next Step
Your base operating system, lightweight desktop, and core tools are now fully configured.

➡️ **[Step 2: Dependencies](./2-dependencies.md)**










