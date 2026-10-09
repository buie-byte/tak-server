# Step 4: Certificate Generation

This section covers the step-by-step process of creating your server's security certificates and trusted environment.

> [!NOTE]
> All generated Certificate Authority (CA) truststores and certificates will be stored in the following directory: `/opt/tak/certs/files`

---

## 4.1. Access the Certificate Directory and Configure Metadata

1. **Switch to the Dedicated tak System User:**
   Change your terminal session to the `tak` user to ensure all generated certificate files are created with correct file ownership and system permissions:
   ```bash
   sudo su tak
   ```

2. **Configure Certificate Identity Settings:**
   Open the certificate metadata file in a text editor to define your server's organizational and geographic settings:
   ```bash
   nano /opt/tak/certs/cert-metadata.sh
   ```

3. **Navigate to the Script Directory:**
   ```bash
   cd /opt/tak/certs
   ```

---

## 4.2. Create the Certificate Authority (CA) and Server Certificates

1. **Establish the Root Certificate Authority:**
   Run the root CA generation script to create your top-level trusted certificate:
   ```bash
   ./makeRootCa.sh --ca-name <CAcommonName>
   ```
   *Example:*
   ```bash
   ./makeRootCa.sh --ca-name TAK-ROOT-CA-01
   ```

2. **Generate and Link the Intermediate CA:**
   Create a subordinate, intermediate CA and link it to your newly created root authority:
   ```bash
   ./makeCert.sh ca <CAcommonName>
   ```
   *Example:*
   ```bash
   ./makeCert.sh ca TAK-ID-CA-01
   ```
   *Note: When prompted "Do you want me to move the files around so that future server and client certificates are signed by this new CA? [Y/N]", type `y`.*

3. **Generate the Server Certificate:**
   Create a unique security certificate mapped directly to your server's domain name or static IP address:
   ```bash
   ./makeCert.sh server <commonName>
   ```
   *Example using a domain name:*
   ```bash
   ./makeCert.sh server takserver
   ```
   *Example using an IP address:*
   ```bash
   ./makeCert.sh server 10.3.120.45
   ```

---

## 4.3. Generate Client and Administrative Certificates

1. **Generate Standard Client Certificates:**
   Create individual client certificates for standard ATAK devices on your network:
   ```bash
   ./makeCert.sh client <commonName>
   ```
   *Example:*
   ```bash
   ./makeCert.sh client user
   ```

2. **Generate Administrative Client Certificates:**
   Create a dedicated client certificate for high-privilege administrative management:
   ```bash
   ./makeCert.sh client <commonName>
   ```
   *Example:*
   ```bash
   ./makeCert.sh client admin
   ```

3. **Exit the tak User Session:**
   Return to your normal user terminal session:
   ```bash
   exit
   ```

---

## 4.4. Apply Certificates and Authorize Admin Access

1. **Restart the TAK Server:**
   Restart the server daemon to load and apply your new server certificates:
   ```bash
   sudo systemctl restart takserver
   ```

2. **Authorize the Administrative Certificate:**
   Map the newly generated `admin.pem` certificate directly to the administrator role within the TAK Server database:
   ```bash
   sudo java -jar /opt/tak/utils/UserManager.jar certmod -A /opt/tak/certs/files/admin.pem
   ```
   *Note: Ensure the command output confirms the user updates match the structure below:*
   ```plaintext
   User Updated:
           Username:      'admin'
           Role:          ROLE_ADMIN
           Fingerprint:   12:A8:56:78:91:01:21:FF:D7:12:A8:56:78:91:01:21:FF:D7
           Groups (read and write permission):
                   __ANON__
   ```

---

### Next Step

With your certificates safely created and your administrator credentials authorized, you are ready to configure the server network sockets and client configurations.

➡️ **[Step 5: Final Configuration & Client Setup](./5-Final-Config.md)**
