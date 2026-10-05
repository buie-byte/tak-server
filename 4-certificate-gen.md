# Certificate generation
This section simplifies the step-by-step process of creating your server's security certificates and trusted environment.

>🛈 Note: The generated CA truststores and certs will be located here: `/opt/tak/certs/files`.

---


Switch your terminal session to the dedicated `tak` system user to ensure all generated certificates are created with the correct file ownership and permissions:
```
sudo su tak
```

Open the metadata configuration file in a text editor to define your server's geographic and organizational identity settings (such as country, state, and organization name):
```
nano /opt/tak/certs/cert-metadata.sh
```

Navigate directly into the target certificates directory where the built-in certificate generation scripts must be executed:
```
cd /opt/tak/certs
```

Run the root authority script to establish your private, top-level Certificate Authority (CA) that will cryptographically anchor all other certificates on your network:
```
./makeRootCa.sh --ca-name <CAcommonName>
```
>🛈 Example:
><br>./make RootCa.sh –-ca-name TAK-ROOT-CA-01

Run the certificate script to generate a subordinate, intermediate CA and link it to your newly created root authority:
```
./makeCert.sh ca <CAcommonName>
```
>🛈 Example:
><br>./make RootCa.sh –-ca-name TAK-ID-CA-01
><br>
><br>Follow the prompt to name the intermediate CA. When prompted *Do you want me to move the files around so that future server and client certificates are signed by this new CA? [Y/N]*, type `y` as this is our desired outcome.

Generate a unique security certificate assigned directly to your server's domain name or IP address to encrypt all incoming connection traffic:
```
./makeCert.sh server <commonName>
```
>🛈 Example using domain name:
><br>./makeCert server takserver
><br>
><br>🛈 Example using IP address:
><br>./makeCert server 10.3.120.45

Create an individual security certificate for standard ATAK devices on your network to securely authenticate them to the server:
```
./makeCert.sh client <commonName>
```
>🛈 Example:
><br>./makeCert client user

Generate a dedicated administrative client certificate that grants secure, high-privilege access for managing your TAK Server:
```
./makeCert.sh client <commonName>
```
>🛈 Example:
><br>./makeCert client admin

Become a normal user:
```
exit
```

Restart the TAK Server:
```
sudo systemctl restart takserver
```

Authorize the admin cert to perform administrative functions using the UI:
```
sudo java -jar /opt/tak/utils/UserManager.jar certmod -A /opt/tak/certs/files/admin.pem
```
> 🛈 Note: You **must** receive a confirmation that looks similar to this:
> <br>
<br>User Updated:
<br>&emsp;&emsp;&emsp;Username:&emsp;&emsp;'admin'
<br>&emsp;&emsp;&emsp;Role:&emsp;&emsp;ROLE_ADMIN
<br>&emsp;&emsp;&emsp;Fingerprint:&emsp;&emsp;12:A8:56:78:91:01:21:FF:D7:12:A8:56:78:91:01:21:FF:D7
<br>&emsp;&emsp;&emsp;Groups (read and write permission):
<br>&emsp;&emsp;&emsp;&emsp;\_\_ANON__
