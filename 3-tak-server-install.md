# Step 3: TAK Server Installation

This guide covers the core installation process of the TAK Server software. Please complete these steps in the exact order presented.

---

## 3.1. Download Required Files

1. Download the `.key`, `.pol`, and `.deb` files from the official [tak.gov](https://tak.gov/products/tak-server) portal to your local Downloads folder:
   * `takserver-public-gpg.key`
   * `deb_policy.pol`
   * `takserver_x.x-RELEASExx_all.deb`

---

## 3.2. Verify GPG Signature and Package Integrity

1. **Install the Verification Utility:**
   ```bash
   sudo apt install debsig-verify -y
   ```

2. **Retrieve Your Unique GPG Key ID:**
   Open your downloaded `deb_policy.pol` file to locate your unique GPG Key ID.
   
> [!NOTE]
> For the following commands, you must replace the placeholder ID `039FCDA2D8907527` with your actual GPG Key ID.

3. **Create the Verification Directories:**
   Create the secure directories required to store your public key ring and signature policies:
   ```bash
   sudo mkdir -p /usr/share/debsig/keyrings/039FCDA2D8907527
   sudo mkdir -p /etc/debsig/policies/039FCDA2D8907527
   ```

4. **Initialize the Empty Keyring File:**
   ```bash
   sudo touch /usr/share/debsig/keyrings/039FCDA2D8907527/debsig.gpg
   ```

5. **Import the Public Key:**
   Navigate to your Downloads folder and import the official public GPG security key into the keyring:
   ```bash
   cd ~/Downloads
   sudo gpg --no-default-keyring --keyring /usr/share/debsig/keyrings/039FCDA2D8907527/debsig.gpg --import takserver-public-gpg.key
   ```

6. **Deploy the Policy Configuration:**
   Copy the signature policy configuration file into the policies directory and rename it to `debsig.pol` so the system can read it:
   ```bash
   sudo cp deb_policy.pol /etc/debsig/policies/039FCDA2D8907527/debsig.pol
   ```

7. **Perform Package Verification:**
   Run the verification tool in verbose mode to confirm the signature matches your imported key and policy. Replace the filename placeholder with your actual `.deb` file name:
   ```bash
   debsig-verify -v takserver_x.x-RELEASExx_all.deb
   ```
   *Verify that the command output contains the following statement:*
   `debsig: Verified package from 'TAK Product Center' (TAK Server Release)`

---

## 3.3. Install and Initialize TAK Server

1. **Update System Repositories:**
   Ensure your local package index is completely up to date before initiating the installation:
   ```bash
   sudo apt update
   ```

2. **Install the TAK Server Package:**
   Install the verified Debian package. The package manager will automatically fetch and resolve any required dependencies:
   ```bash
   cd ~/Downloads
   sudo apt install ./takserver_x.x-RELEASExx_all.deb -y
   ```

3. **Reload System Daemons:**
   Force systemd to reload its background configurations to recognize the newly installed TAK Server service:
   ```bash
   sudo systemctl daemon-reload
   ```

4. **Enable the Service on System Boot:**
   Configure the system to launch the TAK Server service automatically during startup:
   ```bash
   sudo systemctl enable takserver
   ```

5. **Start the TAK Server Process:**
   Bring your TAK Server online immediately:
   ```bash
   sudo systemctl start takserver
   ```

6. **Verify Service Status:**
   Check the active running state of the service to verify that it successfully launched without errors:
   ```bash
   sudo systemctl status takserver --no-pager
   ```

---

### Next Step

With the core TAK Server software successfully installed and running, you are ready to configure the internal security enclave and generate certificates.

➡️ **[Step 4: Certificate Generation](./4-certificate-gen.md)**
