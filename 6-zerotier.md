# Step 6: ZeroTier Installation

This section covers installing and configuring ZeroTier One to connect your node to a secure, private virtual network.

> [!NOTE]
> Ensure you have created a ZeroTier account before starting this process. ZeroTier offers up to 10 free device licenses on standard networks.

---

## 6.1. Install ZeroTier One

1. **Run the Official Installation Script:**
   ```bash
   curl -s https://install.zerotier.com | sudo bash
   ```

2. **Verify the Daemon Service Status:**
   Confirm that the background service is active and running:
   ```bash
   sudo systemctl status zerotier-one --no-pager
   ```
   *Expected Output snippet:*
   ```plaintext
   ● zerotier-one.service - ZeroTier One
        Loaded: loaded (/lib/systemd/system/zerotier-one.service; enabled; vendor preset: enabled)
        Active: active (running) since ...
   ```

3. **Identify Your Node ID:**
   Verify that your local daemon is responding and record your unique **10-digit Node ID**:
   ```bash
   sudo zerotier-cli info
   ```
   *Expected Output snippet:*
   ```plaintext
   200 info 1a2b3c4d5e 1.12.2 ONLINE
   ```

4. **Join Your Virtual Network:**
   Replace `<YOUR_16_DIGIT_NETWORK_ID>` with your actual ZeroTier Network ID to join the network:
   ```bash
   sudo zerotier-cli join <YOUR_16_DIGIT_NETWORK_ID>
   ```
   *Expected Output snippet:*
   ```plaintext
   200 join OK
   ```

---

## 6.2. Authorize the Node on Your Network

Although your node has joined the network, traffic will not route until the connection is approved by the network controller.

1. **Access the Console:**
   Log into your ZeroTier Central Console (or contact your network administrator).

2. **Navigate to Network Members:**
   Open your network configuration page and scroll down to the **Members** section.

3. **Locate Your Node:**
   Find the row matching the **10-digit Node ID** you recorded in Step 6.1.3.

4. **Approve the Connection:**
   Check the **Auth?** checkbox to approve the device. Once authorized, the controller will automatically assign a virtual managed IP address to your device.

---

## 6.3. Verify the Network Connection

1. **Confirm Status and IP Address Allocation:**
   Check your node's network table to ensure it is active and has received its virtual IP address:
   ```bash
   sudo zerotier-cli listnetworks
   ```

2. **Validate Network Sockets:**
   Confirm the output matches the following parameters:
   * **Status:** Must read `OK` (not `ACCESS_DENIED` or `REQUESTING_CONFIGURATION`).
   * **IP Address:** A virtual network interface (typically starting with `zt`) must display your newly assigned managed IP address.

---

### End of Guide

You have successfully completed the TAK Server installation, security configuration, client mapping, and private network integration.

⬅️ **[Return to Main README](./README.md)**
