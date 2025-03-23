# Apache NiFi Installation on Ubuntu 20.04

This guide provides a step-by-step approach to installing **Apache NiFi** on Ubuntu 20.04.

## 🚀 Step 1 - Prepare the Environment

### Install Java 11
NiFi requires Java 11. You can follow this guide to install Java:  
[Java Installation Guide](https://github.com/ozgunakin/java-installation-on-ubuntu20.04)

### Create a Download Directory
To organize the installation files, create a directory:

```bash
mkdir ~/downloads
cd ~/downloads
```

---

## 🔽 Step 2 - Download Apache NiFi

Before proceeding, check the latest NiFi version at:  
[Apache NiFi Download Page](https://nifi.apache.org/download.html)

Then download the latest stable version (adjust the version if needed):

```bash
wget https://archive.apache.org/dist/nifi/1.15.0/nifi-1.15.0-bin.tar.gz
```

---

## 📦 Step 3 - Extract & Move NiFi Files

Extract the downloaded NiFi archive:

```bash
sudo tar -zxvf nifi-1.15.0-bin.tar.gz
```

Move the extracted files to your preferred installation directory (`/opt/` recommended):

```bash
sudo mv nifi-1.15.0 /opt/nifi
```

---

## ⚙️ Step 4 - Configure NiFi Properties

Navigate to the configuration directory:

```bash
cd /opt/nifi/conf
```

Edit the `nifi.properties` file:

```bash
sudo nano nifi.properties
```

Modify the following lines to set the **web host and port**:

```properties
nifi.remote.input.secure=false  # Must be false
nifi.web.http.host=0.0.0.0  # Allows access from any IP
nifi.web.http.port=8080  # Change this if needed
nifi.remote.input.http.enabled= false

nifi.web.https.host=
nifi.web.https.port=
```

🔹 If installing **NiFi 1.15.3**, clear the following security settings:

```properties

nifi.security.keystore= 
nifi.security.keystoreType= 
nifi.security.keystorePasswd= 
nifi.security.keyPasswd= 
nifi.security.truststore= 
nifi.security.truststoreType= 
nifi.security.truststorePasswd=
```

Save and exit: **CTRL + X → Y → Enter**

---

## 🔧 Step 5 - Install NiFi as a Service

Navigate to the NiFi directory:

```bash
cd /opt/nifi
```

Install NiFi as a system service:

```bash
sudo ./bin/nifi.sh install
```

Reload the system daemon to apply the changes:

```bash
sudo systemctl daemon-reload
```

---

## ▶️ Step 6 - Start NiFi Service

Start NiFi using the system service:

```bash
sudo service nifi start
```

Check if NiFi is running:

```bash
sudo service nifi status
```

If everything is correct, you should see **NiFi running**.

---

## 🌐 Step 7 - Access NiFi Web UI

Open a browser and go to:

```
http://your-ip-address:8080/nifi
```

If using **localhost**, use:

```
http://127.0.0.1:8080/nifi
```

---

### ✅ **Final Notes**
- If NiFi fails to start, check logs:
  ```bash
  tail -f /opt/nifi/logs/nifi-app.log
  ```
- Ensure port **8080** is open:
  ```bash
  sudo ufw allow 8080/tcp
  ```
- If accessing remotely, replace `your-ip-address` with the actual **server IP**.

**You have successfully installed Apache NiFi on Ubuntu 20.04.** 🎉

