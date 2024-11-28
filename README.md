
# FiveM Debian Installation Guide

This guide provides step-by-step instructions for installing and configuring a custom FiveM server on a Debian-based system, using an Ubuntu virtual machine (VM).

---

## Table of Contents

1. [VM Installation Guide](#vm-installation-guide)  
2. [Ubuntu Installation Guide](#ubuntu-installation-guide)  
3. [FiveM Setup and Configuration](#fivem-setup-and-configuration)  
4. [Starting Your Server](#starting-your-server)  
5. [Configuring txAdmin](#configuring-txadmin)  
6. [Optional: Configuring Linux Service](#optional-configuring-linux-service)

---

## VM Installation Guide

If you're already familiar with VM setup, you can skip this section.

### Getting Started
#### Prerequisites
1. **Ubuntu Server ISO File**  
   Download the ISO file from [Ubuntu Server](https://ubuntu.com/download/server).  

2. **Virtualization Software**  
   Choose any of the following:  
   - [VMware](https://www.youtube.com/watch?v=PoNPBdKLZdk)  
   - [Hyper-V](https://www.youtube.com/watch?v=FCIA4YQHx9U)  
   - [VirtualBox](https://www.youtube.com/watch?v=8mns5yqMfZk)  

3. **Create a VM Instance**  
   Use one of the guides above to create a VM instance with the Ubuntu ISO.

---

## Ubuntu Installation Guide
1. Select your language and keyboard layout.
2. Update the installer.
3. Configure the network (note the machine’s IP address).
4. Skip proxy configuration and Ubuntu mirror adjustments (unless outside the US).
5. Configure storage: use the entire disk unless a custom layout is needed.
6. Confirm disk partitioning.
7. Set up a user profile (username and password).
8. Configure OpenSSH (recommended for production servers).  
9. Skip installing additional snaps.  
10. Complete the installation and reboot.

---

## FiveM Setup and Configuration

### Connect to Your Server
1. Use an SSH client (e.g., [PuTTY](https://www.putty.org/)) to connect to your server:  
   - Enter the server’s IP address.  
   - Log in with the username and password created during setup.

2. Update server packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

3. Install essential tools:
   ```bash
   sudo apt install git nano xz-utils -y
   ```

---

### Configure MariaDB
1. Install MariaDB:
   ```bash
   sudo apt install mariadb-server -y
   ```

2. Secure the installation:
   ```bash
   sudo mysql_secure_installation
   ```
   - Follow prompts (use secure settings as detailed above).  

3. Create a database user:
   ```bash
   sudo mysql
   create user 'fivem'@'%' identified by 'securepassword';
   grant all privileges on *.* to 'fivem'@'%';
   flush privileges;
   exit;
   ```

4. Restart MariaDB:
   ```bash
   sudo systemctl restart mariadb
   ```

---

### Install FiveM Server
1. Create a directory:
   ```bash
   mkdir -p ~/FXServer/server
   cd ~/FXServer/server
   ```

2. Download and extract the latest FiveM server files [FiveM Linux Artifacts](https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/):
   ```bash
   wget <FiveM_Server_Download_Link>
   tar xf fx.tar.xz
   rm fx.tar.xz
   ```

3. Obtain a server license key from [FiveM Keymaster](https://keymaster.fivem.net/).

---

## Starting Your Server
You can use `screen` or configure a Linux service to run your server.

### Using `screen`:
1. Install `screen`:
   ```bash
   sudo apt install screen -y
   ```

2. Run the server:
   ```bash
   screen -S fivem
   cd ~/FXServer/server
   bash ~/FXServer/server/run.sh
   ```

---

## Configuring txAdmin
1. Access txAdmin by visiting your server’s IP and port (e.g., `10.0.0.231:40120`).  
2. Link your cfx.re account and complete the setup:
   - Enter a backup password.
   - Configure your server name, recipe, and database options.

---

## Optional: Configuring Linux Service
1. Create a service file:
   ```bash
   cd /etc/systemd/system
   sudo nano fivem.service
   ```

2. Add the following content:
   ```bash
   [Service]
   ExecStart=/home/<username>/FXServer/server/run.sh
   Restart=always
   User=<username>
   Group=<groupname>
   WorkingDirectory=/home/<username>/FXServer/server
   ```

3. Enable and start the service:
   ```bash
   sudo systemctl enable fivem.service
   sudo systemctl start fivem.service
   ```

---

Feel free to let me know if you’d like additional tweaks or help inserting the `<username>` placeholders!
