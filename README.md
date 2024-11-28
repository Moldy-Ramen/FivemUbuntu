
# FivemDebian  
**Custom FiveM Debian Installation Guide**

## VM Installation Guide  
*If you already know how to install and launch a virtual machine (VM), skip this section.*  

### Getting Started  
### Requirements:  
1. **Ubuntu Server ISO File**  
   - Download the ISO file [here](https://ubuntu.com/download/server).  

2. **Virtualization Software**  
   - Choose one of the following options:  
     - [VMWare Installation Guide](https://www.youtube.com/watch?v=PoNPBdKLZdk)  
     - [Hyper-V Installation Guide](https://www.youtube.com/watch?v=FCIA4YQHx9U)  
     - [VirtualBox Installation Guide](https://www.youtube.com/watch?v=8mns5yqMfZk)  

3. **VM Instance**  
   - Use one of the guides above to create a VM instance with the Ubuntu ISO.  
   - *Note: Detailed instructions for creating a VM instance are omitted due to the quality of the provided guides.*  

---

## Ubuntu Installation Guide  

1. **Select Language**  
   - Use the arrow keys to choose a language and press `Enter`.  

2. **Update Installer**  
   - Select **Update the Installer**, then wait for the update to complete.  

3. **Choose Keyboard Layout**  
   - Select your desired keyboard layout.  

4. **Configure Network**  
   - Do not change any settings. Make a note of your machine’s IP address; you’ll need it later.  

5. **Skip Proxy Configuration**  
   - Ensure **Done** is selected, then press `Enter`.  

6. **Configure Ubuntu Mirror**  
   - If in the US, select **Done** to keep the default.  
   - For other regions, find a local Ubuntu mirror from [here](https://launchpad.net/ubuntu/+archivemirrors) and update accordingly.  

7. **Configure Storage**  
   - Use the entire disk by selecting **Done**, or choose a custom layout to create your own partitions.  

8. **Partition Summary**  
   - Review the partitions and select **Done**.  

9. **Confirm Destructive Action**  
   - Select **Continue** to partition the disk and begin the installation.  

10. **Configure Profile**  
    - Complete the form with your name, preferred server name, and user account settings (username and password).  

11. **Configure SSH**  
    - Select **OpenSSH**.  
    - *Optional:* For production servers, importing an SSH key is recommended for additional security.  

12. **Skip Featured Server Snaps**  
    - Select **Done** to proceed without additional packages.  

13. **Reboot**  
    - Ubuntu will install, and upon completion, reboot the system.  

---

## FiveM Setup with Auto Updates  

*If you don’t want to configure auto-updates, skip to the regular installation section below.*  

1. **Log in to Your Server**  
   - Use your preferred SSH client (e.g., PuTTY).  
   - *Beginner Tip:* Download and install [PuTTY](https://www.putty.org/) for simplicity.  

     Steps to connect:  
     - Open PuTTY.  
     - Enter your server's IP address in the **Hostname** field.  
     - (Optional) Save your server details in the **Saved Sessions** box.  
     - Press **Open**, accept the SSH fingerprint, and log in with the username and password set during setup.  

2. **Update Server Repositories and Software**  
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```  

3. **Install Git, xz, and Nano**  
   ```bash
   sudo apt install xz git nano -y
   ```  

4. **Create Directory for Auto-Update Script**  
   ```bash
   mkdir autoupdate
   ```  

---
