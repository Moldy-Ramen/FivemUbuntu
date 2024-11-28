
# FiveM Debian Setup Guide
A custom installation guide for setting up a FiveM server on a Debian-based VM.

## Getting Started
### Prerequisites
1. **Ubuntu Server ISO File**
   - Download the ISO file [here](https://ubuntu.com/download/server).

2. **Virtualization Software**
   - Choose one of the following:
     - [VMware Install Guide](https://www.youtube.com/watch?v=PoNPBdKLZdk)
     - [Hyper-V Install Guide](https://www.youtube.com/watch?v=FCIA4YQHx9U)
     - [VirtualBox Install Guide](https://www.youtube.com/watch?v=8mns5yqMfZk)

3. **VM Instance**
   - Use the provided guides to create a VM instance with the Ubuntu Linux ISO.

---

## Ubuntu Installation
1. **Language Selection**  
   Use the arrow keys to select your language and press **Enter**.
2. **Update Installer**  
   Select "Update the Installer" and wait for the process to complete.
3. **Keyboard Layout**  
   Choose your preferred layout.
4. **Network Configuration**  
   Make note of the machine's IP address; you’ll need it later.
5. **Proxy Configuration**  
   Skip this step by selecting **Done**.
6. **Ubuntu Mirror**  
   - If in the US, skip this step and select **Done**.  
   - Otherwise, find your local mirror [here](https://launchpad.net/ubuntu/+archivemirrors).
7. **Storage Configuration**  
   - To use the entire disk, select **Done**.
   - For custom partitions, configure as needed.
8. **Partition Summary**  
   Confirm by selecting **Done**.
9. **Partition Confirmation**  
   Select **Continue** to partition the disk and install Ubuntu.
10. **Profile Configuration**  
    Fill out the form with your name, server name, and user credentials.
11. **SSH Configuration**  
    - Select OpenSSH for installation.  
    - For production servers, consider importing an SSH key.
12. **Server Snaps**  
    Skip by selecting **Done**.
13. **Finalize Installation**  
    After installation, select **Reboot**.

---

## FiveM Setup and Configuration
1. **Login to Your Server**  
   Use an SSH client (e.g., [Putty](https://www.putty.org/)) to connect:
   - Enter your server's IP address.
   - Login with the credentials created earlier.
2. **Update Server**  
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. **Install Required Packages**  
   ```bash
   sudo apt install git nano xz-utils -y
   ```
4. **MariaDB Installation and Configuration**  
   - Install MariaDB:
     ```bash
     sudo apt install mariadb-server -y
     ```
   - Start the MariaDB service (only required once, as it is enabled by default on startup):
     ```bash
     sudo systemctl start mariadb.service
     ```
   - Secure MariaDB by running:
     ```bash
     sudo mysql_secure_installation
     ```
     Follow these steps:
     1. Enter your MariaDB root password.
     2. At the **Switch to Unix Socket Authentication?** prompt, type `n` and press **Enter**.
     3. At the **Change the root password?** prompt, type `n` and press **Enter**.
     4. At the **Remove anonymous users?** prompt, type `Y` and press **Enter**.
     5. At the **Disallow root login remotely?** prompt, type `Y` and press **Enter**.  
        *(This is crucial for security.)*
     6. At the **Remove test database?** prompt, type `Y` and press **Enter** (optional, but recommended).
     7. At the **Reload privilege tables?** prompt, type `Y` and press **Enter**.
   - Create a user and database for the FiveM server:
     1. Open the MySQL command line:
        ```bash
        sudo mysql
        ```
     2. Create a user with a secure password (replace `password123!` with your own secure password):
        ```bash
        create user 'fivem'@'%' identified by 'password123!';
        ```
     3. Grant all privileges to the `fivem` user:
        ```bash
        grant all privileges on *.* to 'fivem'@'%';
        ```
     4. Flush the privilege changes:
        ```bash
        flush privileges;
        ```
     5. Exit the MySQL command line:
        ```bash
        exit
        ```
     6. Restart MariaDB to apply changes:
        ```bash
        sudo systemctl restart mariadb
        ```

5. **FXServer Setup**  
   - Create and enter the server directory:
     ```bash
     mkdir -p ~/FXServer/server
     cd ~/FXServer/server
     ```
   - Download the latest server version:
     ```bash
     wget <server_download_url>
     tar xf fx.tar.xz
     rm fx.tar.xz
     cd ../server-data
     ```
   - Obtain a server license key [here](https://keymaster.fivem.net).

---

## Running Your Server
### Using `screen`:
1. Install `screen`:
   ```bash
   sudo apt install screen -y
   ```
2. Run the server:
   ```bash
   screen -S fivem
   cd ~/FXServer/server
   bash run.sh
   ```

### Configuring txAdmin:
1. Open `http://<your_vm_ip>:40120` in your browser.
2. Link your CFX.re account using the provided PIN.
3. Complete the setup:
   - Backup password.
   - Server name.
   - Recipe and template selection.
   - Server location:
     ```bash
     /home/<your_username>/FXServer/server-data
     ```
   - Add your server license key and database credentials.

---

## Optional: Configure a Linux Service
1. Create a service file:
   ```bash
   sudo nano /etc/systemd/system/fivem.service
   ```
2. Paste the following:
   ```ini
   [Unit]
   Description=FiveM Server Service

   [Service]
   Type=simple
   ExecStart=/home/<your_username>/FXServer/server/run.sh
   Restart=always

   [Install]
   WantedBy=default.target
   ```
3. Enable and start the service:
   ```bash
   sudo systemctl enable fivem.service
   sudo systemctl start fivem.service
   ```

---

## Celebrate!
Your server is now live and ready to go!
