# Final Configuration & Client Setup
This page covers the final steps to make the server operational and connect clients.

---

## Configure Uncomplicated Firewall (UFW)

Install the Uncomplicated Firewall (UFW) management tool:

```
sudo apt install ufw
```

Reload the firewall's configuration:
```
sudo ufw reload
```
>⚠︎ Warning: For Raspberry Pi OS installs, you must reboot your device after installing ufw. 

Check the current operational status and list of active rules for your firewall:
```
sudo ufw status
```

Set the firewall's default behavior to block all incoming network connections for enhanced security:
```
sudo ufw default deny incoming
```

Configure the firewall's default behavior to permit all outbound network connections from your server:
```
sudo ufw default allow outgoing
```

Create a specific rule to allow incoming SSH traffic so you can maintain remote access to your server:
```
sudo ufw allow ssh
```

Activate the firewall service to begin enforcing its rules:
```
sudo ufw enable
```

Open port 8089 to allow incoming traffic required for TAK Server communications:
```
sudo ufw allow 8089
```

Open port 8443 to allow secure (SSL/TLS) incoming traffic for TAK Server communications:
```
sudo ufw allow 8443
```

## Configure TAK Server Certificate

First, go to your TAK Server configuration:
```
cd /opt/tak
```

Open CoreConfig.xml:
```
sudo nano CoreConfig.xml
```

Now you are checking two areas of this file: `<security>` and `<network>`.

A. Check the `<security>` section

Find the `<security>` section. Inside it should be a `<tls ... />` entry similar to:

`<security>
    <br><tls
        context="TLSv1" keymanager="SunX509" keystore="JKS"
        keystoreFile="certs/files/takserver.jks" keystorePass="atakatak"
        truststore="JKS" truststoreFile="certs/files/truststore-TAK-ID-CA-01.jks"
        truststorePass="atakatak"
    />
</security>`


## Install Client Certificates on ATAK
