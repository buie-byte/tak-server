# TAK Server Installation Guide (Ubuntu / Raspberry Pi)

This guide provides a comprehensive, step-by-step process for installing a fully functional TAK Server on Raspberry Pi w/ Ubuntu Server 22.04.

---

## Installation Process

Please follow the steps in order. Each section is organized as a separate "tab" in this guide.

| **Tab** | **Description** |
|:---|:---|
| **[Step 1: Initial Server Setup](./1-intial-setup.md)** | Prepare the OS, install a lightweight GUI, and update the system. |
| **[Step 2: Dependencies & Pre-Install Config](./2-dependencies.md)** | Install Java, configure system limits, and set up the PostgreSQL database. |
| **[Step 3: TAK Server Installation](./3-tak-server-install.md)** | Download, verify, and install the official TAK Server package. |
| **[Step 4: Certificate Generation](./4-certificate-gen.md)** | Create the Certificate Authority (CA) and generate server/client certificates. |
| **[Step 5: Final Configuration & Client Setup](./5-final-config.md)** | Configure the firewall, apply certificates, and set up your ATAK clients. |
| **[Step 6: ZeroTier Setup](./6-zerotier.md)** | Configure ZeroTier for a secure, private VPN. |

---

## License & Disclaimer

This project is licensed under the MIT License - see the `LICENSE` file for details.

**Disclaimer:** This repository is an independent installation guide and is not affiliated with, endorsed by, or connected to the Department of Defense, the TAK Product Center, or any official TAK development entity. Use these instructions at your own risk.
