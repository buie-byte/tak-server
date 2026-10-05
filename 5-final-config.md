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

First, find the `<security>` section. Inside it should be a `<tls ... />` entry similar to:

`<security>`
<br>&emsp;`<tls context="TLSv1" keymanager="SunX509" keystore="JKS" keystoreFile="certs/files/takserver.jks" keystorePass="atakatak" truststore="JKS" truststoreFile="certs/files/truststore-TAK-ID-CA-01.jks" truststorePass="atakatak">`
<br>`</security>`
> 🛈 Note:
> <br> If you are using a Certificate Revocation List (CRL), uncomment the following:
<br>`<!-- <crl _name="Marti CA" crlFile="certs/ca.crl"/> -->`


Second, change the `keystoreFile` attribute to the server keystore that you newly created with `makeCerts.sh server <commonName>`. 
> 🛈 Example:
>  <br>certs/files/takserver.jks or your specific server IP.jks

Third, change the `truststoreFile` attribute to the trust store you newly created with `makeCert.sh ca <CAcommonName>` 
> 🛈 Example:
> <br>certs/files/truststore-TAK-ID-CA-01.jks

Next, find the `<network>` section. Inside it should be an entry similar to:

`<network multicastTTL="5">`
<br>&emsp;`<input _name="stdtcp" protocol="tcp" port="8087"/>`
<br>&emsp;`<input _name="stdudp" protocol="udp" port="8087"/>`
<br>&emsp;`<input _name="stdssl" protocol="tls" port="8089" auth="x509"/>`
<br>&emsp;`<input _name="streamtcp" protocol="stcp" port="8088"/>`
<br>&emsp;`<input _name="SAproxy" protocol="mcast" group="239.2.3.1" port="6969" proxy="true"/>`
<br>&emsp;`<input _name="GeoChatproxy" protocol="mcast" group="224.10.10.1" port="17012" proxy="true"/>`
<br>`</network>`

Add a TLS input specifying group-based filtering:

```
<input _name="stdssl" protocol="tls" port="8089" auth="x509"/>
```



## Install Client Certificates on ATAK
