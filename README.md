# CentOS 9 Server Setup with VMware and mRemoteNG

This guide provides step-by-step instructions to set up a CentOS 9 server on VMware Workstation, configure networking (including changing the connection name), enable SSH, and connect using mRemoteNG.

---

## **Contents**

- [VMware Workstation Installation](#vmware-workstation-installation)
- [CentOS 9 Installation](#centos-9-installation)
- [NMCLI Configuration](#nmcli-configuration)
- [Change Connection Name](#change-connection-name)
- [Enable NMCLI](#enable-nmcli)
- [Switch to Root User](#switch-to-root-user)
- [SSH Installation and Configuration](#ssh-installation-and-configuration)
- [Enable SSH](#enable-ssh)
- [Firewall Setup](#firewall-setup)
- [Check IP Address, Hostname, and User](#check-ip-address-hostname-and-user)
- [mRemoteNG Setup](#mremoteng-setup)

---

### **VMware Workstation Installation**

1. Download and install VMware Workstation from [VMware’s official website](https://www.vmware.com/products/workstation-pro.html).
2. Ensure your system meets the software requirements.

---

### **CentOS 9 Installation**

1. Download the CentOS 9 ISO from the [CentOS website](https://www.centos.org/).
2. Open VMware Workstation and create a new virtual machine:
   - Select **Typical (recommended)**.
   - Attach the CentOS 9 ISO.
   - Set up disk size (at least 20 GB) and memory (2 GB+ recommended).
   - Choose **Bridged Networking** or **NAT**.
3. Start the VM and follow the CentOS installation wizard:
   - Select **Server (Minimal or GUI)** installation.
   - Set the root password and create a new user.

---

### **NMCLI Configuration**

1. List all available connections:
   ```bash
   nmcli con show
   ```
2. Activate the desired connection:
   ```bash
   nmcli con up "<connection_name>"
   ```
3. To use DHCP:
   ```bash
   sudo vi /etc/sysconfig/network-scripts/ifcfg-<interface_name>
   ```
   Example configuration for DHCP:
   ```plaintext
   TYPE=Ethernet
   BOOTPROTO=dhcp
   DEVICE=ens33
   ONBOOT=yes
   ```

---

### **Change Connection Name**

1. Find the current connection name:
   ```bash
   nmcli con show
   ```
   Example output:
   ```plaintext
   NAME                UUID                                  TYPE      DEVICE
   Wired connection 1  12345678-90ab-cdef-1234-567890abcdef  ethernet  ens33
   ```

2. Change the connection name:
   ```bash
   nmcli con modify "Wired connection 1" connection.id "new_connection_name"
   ```

3. Verify the change:
   ```bash
   nmcli con show
   ```

---

### **Enable NMCLI**

1. Restart the NetworkManager service:
   ```bash
   sudo systemctl restart NetworkManager
   ```
2. Check the connection status:
   ```bash
   nmcli device status
   ```

---

### **Switch to Root User**

To switch to the root user:
```bash
su -
```
Enter the root password when prompted.

---

### **SSH Installation and Configuration**

1. Install OpenSSH server:
   ```bash
   sudo dnf install openssh-server
   ```
2. Start and enable the SSH service:
   ```bash
   sudo systemctl start sshd
   sudo systemctl enable sshd
   ```

---

### **Enable SSH**

1. Allow SSH through the firewall:
   ```bash
   sudo firewall-cmd --permanent --add-service=ssh
   sudo firewall-cmd --reload
   ```
2. Verify the SSH service is running:
   ```bash
   sudo systemctl status sshd
   ```

---

### **Firewall Setup**

1. Check the current firewall rules:
   ```bash
   sudo firewall-cmd --list-all
   ```
2. Add custom rules if necessary (e.g., allow a specific port or service):
   ```bash
   sudo firewall-cmd --permanent --add-port=22/tcp
   sudo firewall-cmd --reload
   ```

---

### **Check IP Address, Hostname, and User**

1. Get the IP address:
   ```bash
   ip a
   ```
2. Check the hostname:
   ```bash
   hostname
   ```
3. List the current users:
   ```bash
   whoami
   ```

---

### **mRemoteNG Setup**

1. Download and install mRemoteNG from [mRemoteNG.org](https://mremoteng.org).
2. Open mRemoteNG and create a new connection:
   - **Protocol**: SSH v2
   - **Hostname**: IP address of the CentOS server
   - **Port**: 22
   - **Username**: Non-root user created during installation.
3. Save the configuration and connect.

---

## **Troubleshooting**

- **No network connectivity**:
  - Check if the network adapter in VMware is set to **Bridged** or **NAT**.
  - Restart NetworkManager:  
    ```bash
    sudo systemctl restart NetworkManager
    ```
- **SSH connection fails**:
  - Verify the SSH service is running:  
    ```bash
    sudo systemctl status sshd
    ```
  - Ensure the firewall allows SSH traffic.

---

This documentation includes the steps for renaming network connections, enabling SSH, and connecting using mRemoteNG. Let me know if more details are required!
