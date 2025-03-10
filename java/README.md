# Java Installation and PATH Configuration on Ubuntu

## 1. Install Java

First, update your system packages and install Java:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

---

## 2. Find the Java Path

To find the installed Java version and its path, run:

```bash
sudo update-alternatives --config java
```

This command will list all available Java versions on your system. You should see an output similar to:

```
/usr/lib/jvm/java-17-openjdk-amd64/bin/java
```

In this case, **JAVA\_HOME** should be set to:

```
/usr/lib/jvm/java-17-openjdk-amd64
```

---

## 3. Set JAVA\_HOME and PATH Variables

### **3.1 Define Environment Variables**

To ensure Java runs correctly, set **JAVA\_HOME** and **PATH** variables. Open the **.bashrc** file:

```bash
nano ~/.bashrc
```

Add the following lines at the end of the file (Update the path according to your system!):

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

**Save the file:**

- Press **CTRL + X**
- Press **Y** to confirm saving
- Press **Enter** to exit

### **3.2 Apply Changes**

To apply the changes, run:

```bash
source ~/.bashrc
```

**For Zsh users:**

```bash
source ~/.zshrc
```

---

## 4. Verify Path Configuration

### **4.1 Check JAVA\_HOME Variable**

```bash
echo $JAVA_HOME
```

It should display the correct Java path, for example:

```
/usr/lib/jvm/java-17-openjdk-amd64
```

### **4.2 Check Java Version**

```bash
java -version
```

The output should be similar to:

```
openjdk version "17.0.9" 2024-01-16
OpenJDK Runtime Environment (build 17.0.9+11)
OpenJDK 64-Bit Server VM (build 17.0.9+11, mixed mode)
```

If you have followed all the steps correctly, Java should now be successfully installed and configured on your Ubuntu system!

