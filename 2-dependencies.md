# Step 2: Dependencies and Pre-Installation Configuration

This page focuses on installing and configuring the necessary prerequisites for TAK Server itself. Please complete these steps in the exact order presented.

---

## 2.1. Install Java OpenJDK 17

TAK Server relies on Java 17 to run. Follow these steps to verify and install the correct Java Runtime Environment (JRE).

1. **Check for an Existing Java Installation:**
   ```bash
   java --version
   ```

2. **Install OpenJDK 17:**  
   If Java is missing, or if the output of the previous command does not show version 17, run the following command:
   ```bash
   sudo apt update && sudo apt install openjdk-17-jre -y
   ```

---

## 2.2. Increase TCP Connection Limits

TAK Server utilizes a high number of Java threads to maintain active client connections. To prevent the operating system from throttling connections, you must increase the maximum open file limits.

1. **Apply System Limits Configuration:**  
   Execute the following command block to append the updated limits to your system configuration file:
   ```bash
   cat <<'HERE' | sudo tee --append /etc/security/limits.conf > /dev/null
   * soft nofile 32768
   * hard nofile 32768
   HERE
   ```

---

## 2.3. Install PostgreSQL and PostGIS

TAK Server requires a PostgreSQL database paired with the PostGIS spatial extension. Because default system repositories may contain outdated database versions, you must configure the official PostgreSQL repository.

1. **Install the Linux Standard Base (LSB) Tool:**  
   Identify your specific OS distribution release so that the correct database repository is targeted:
   ```bash
   sudo apt install -y lsb-release gnupg2 curl
   ```

2. **Create a Secure Keyring Directory:**  
   Establish a secure directory on your local filesystem to safely store third-party software verification keys:
   ```bash
   sudo mkdir -p /etc/apt/keyrings
   ```

3. **Download the PostgreSQL Public GPG Key:**  
   Fetch the official GNU Privacy Guard (GPG) public key to verify database packages during installation:
   ```bash
   sudo curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc --output /etc/apt/keyrings/postgresql.asc
   ```

4. **Register the PostgreSQL Software Source:**  
   Create a software source configuration file to instruct your package manager to fetch the database files directly from PostgreSQL servers using the downloaded key:
   ```bash
   cat <<HERE | sudo tee /etc/apt/sources.list.d/postgresql.list > /dev/null
   deb [signed-by=/etc/apt/keyrings/postgresql.asc] https://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main
   HERE
   ```

5. **Update Package Lists & Install Database Packages:**  
   Update your local package index to incorporate the newly added PostgreSQL repository, then install PostgreSQL and the PostGIS extension:
   ```bash
   sudo apt update && sudo apt install -y postgresql-15 postgresql-15-postgis-3
   ```

> [!WARNING]
> **Database Versioning:** Ensure you install PostgreSQL version 15 (as shown above) unless your specific TAK Server distribution package explicitly instructs you to utilize a newer version.

---

### Next Step

With your system dependencies, database sources, and performance limits configured, you are ready to install the core TAK Server software.

➡️ **[Step 3: TAK Server Installation](./3-TAK-server-install.md)**
