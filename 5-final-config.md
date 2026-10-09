# Step 5: Final Configuration & Client Setup

This page covers the final steps to make the server operational and connect clients. Please complete these steps in the exact order presented.

---

## 5.1. Configure Uncomplicated Firewall (UFW)

1. **Install the Firewall Utility:**
   ```bash
   sudo apt install ufw -y
   ```

2. **Reload the Firewall Service:**
   ```bash
   sudo ufw reload
   ```
   > [!WARNING]
   > **Raspberry Pi OS Installations:** You must reboot your device after installing UFW before proceeding with further firewall configuration steps.

3. **Check Operational Status:**
   ```bash
   sudo ufw status
   ```

4. **Apply Default Inbound Policy:**
   Configure the firewall to block all incoming network connections by default:
   ```bash
   sudo ufw default deny incoming
   ```

5. **Apply Default Outbound Policy:**
   Configure the firewall to permit all outbound network connections from your server:
   ```bash
   sudo ufw default allow outgoing
   ```

6. **Permit Remote SSH Connections:**
   ```bash
   sudo ufw allow ssh
   ```

7. **Enable the Firewall Service:**
   ```bash
   sudo ufw enable
   ```

8. **Open TAK Communication Ports:**
   Permit incoming traffic on TCP ports 8089 and 8443:
   ```bash
   sudo ufw allow 8089
   sudo ufw allow 8443
   ```

---

## 5.2. Configure TAK Server Certificates in CoreConfig.xml

1. **Navigate to the Server Directory:**
   ```bash
   cd /opt/tak
   ```

2. **Open the Configuration File:**
   ```bash
   sudo nano CoreConfig.xml
   ```

3. **Configure the `<security>` Section:**
   Locate the `<security>` block and modify the `<tls>` entry. Update the `keystoreFile` and `truststoreFile` attributes to match the certificates you generated in Step 4:
   ```xml
   <security>
       <tls context="TLSv1" keymanager="SunX509" keystore="JKS" keystoreFile="certs/files/takserver.jks" keystorePass="atakatak" truststore="JKS" truststoreFile="certs/files/truststore-TAK-ID-CA-01.jks" truststorePass="atakatak">
       </tls>
   </security>
   ```
   *Note: If you are utilizing a Certificate Revocation List (CRL), uncomment the following line by removing the `<!--` and `-->` delimiters:*
   ```xml
   <!-- <crl _name="Marti CA" crlFile="certs/ca.crl"/> -->
   ```

4. **Configure the `<network>` Section:**
   Locate the `<network>` block and append the secure TLS input configuration:
   ```xml
   <network multicastTTL="5">
       <input _name="stdtcp" protocol="tcp" port="8087"/>
       <input _name="stdudp" protocol="udp" port="8087"/>
       <input _name="streamtcp" protocol="stcp" port="8088"/>
       <input _name="SAproxy" protocol="mcast" group="239.2.3.1" port="6969" proxy="true"/>
       <input _name="GeoChatproxy" protocol="mcast" group="224.10.10.1" port="17012" proxy="true"/>
       <input _name="stdssl" protocol="tls" port="8089" auth="x509"/>
   </network>
   ```

5. **Restart the Service to Apply Changes:**
   ```bash
   sudo systemctl restart takserver
   ```

---

## 5.3. Install ATAK Client Certificates on Android

To securely connect your clients to the TAK server, you must install the generated PKCS#12 (.p12) certificates on your devices. Ensure you have obtained `truststore-root.p12` (or your environment's specific intermediate CA `.p12`) and `user.p12` (your individual user certificate) before proceeding.

1. **Transfer Certificates to Device:**
   Copy `truststore-root.p12` and `user.p12` directly to your Android device's local storage (e.g., via USB transfer, secure file transfer, or download folder).

2. **Navigate to Server Settings in ATAK:**
   Open ATAK and navigate to:
   `Settings` -> `Network Preferences` -> `TAK Servers` -> `Menu` (three dots in top right corner) -> `Add`

3. **Configure Connection Properties:**
   * Enter a name for the TAK Server.
   * Enter the server IP Address. (Note: You will configure a private VPN IP using ZeroTier in the next section).
   * Tap `Advanced Options`.
   * Set `Streaming Protocol` to `SSL`.
   * Set `Server Port` to `8089`.

4. **Import the Trust Store Certificate:**
   * Tap `Import Trust Store`.
   * Browse and select your `truststore-root.p12` file.
   * Enter the Trust Store Certificate passphrase.

5. **Import the Client Certificate:**
   * Tap `Import Client Certificate`.
   * Browse and select your `user.p12` file.
   * Enter the Client Certificate passphrase.
   * Tap `OK` to save the server configuration.

---

## 5.4. Install ATAK Admin Certificates on WebTAK

The same `.p12` certificate files are used to secure administrative web-based access to the TAK Server Web UI and WebTAK client.

1. **Open Browser Certificate Settings:**
   * **Chrome / Edge (Windows/macOS):** Go to `Settings` -> `Privacy and Security` -> `Security` -> `Manage Certificates`.
   * **Firefox:** Go to `Settings` -> `Privacy & Security` -> Scroll to `Certificates` -> Click `View Certificates`.

2. **Import the Security Certificates:**
   * Import your `truststore-root.p12` file into the `Authorities` (or `Trusted Root Certification Authorities`) tab.
   * Import your `admin.p12` file into the `Your Certificates` (or `Personal`) tab.
   * Enter the respective certificate passphrases when prompted.

3. **Establish a Secure Connection:**
   * Navigate to your TAK server URL: `https://localhost:8443/Marti`
   * When prompted by your web browser, select the certificate matching your imported `admin.p12` file.

---

### Next Step

Your core TAK Server deployment, firewall protection, certificate mapping, and client software profiles are now fully established.

➡️ **[Step 6: ZeroTier Setup](./6-zerotier.md)**
