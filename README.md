# Fivem Ubuntu
Custom FiveM Ubuntu Installation Guide

## Getting Started with VM Installation

If you're already familiar with installing and launching a VM, you can skip this section.

### Prerequisites
You will need the following:

1. **Ubuntu Server ISO File**  
   Download the ISO file from [Ubuntu's official site](https://ubuntu.com/download/server).

2. **Virtualization Software**  
   Choose one of the following options:
   - [VMware Install Guide](https://www.youtube.com/watch?v=PoNPBdKLZdk)
   - [Hyper-V Install Guide](https://www.youtube.com/watch?v=FCIA4YQHx9U)
   - [VirtualBox Install Guide](https://www.youtube.com/watch?v=8mns5yqMfZk)

3. **VM Instance**  
   Use one of the guides above to create a VM instance with the Ubuntu Server ISO. Detailed instructions for creating a VM instance are omitted here due to the quality of the provided guides.

---

## Ubuntu Installation Guide

1. **Language Selection**  
   Use the arrow keys to select your language, then press `Enter`.

2. **Update Installer**  
   Select "Update the new installer" and wait for the process to complete.

3. **Choose Keyboard Layout**  
   Follow the on-screen instructions to select your preferred keyboard layout.

4. **Configure Network**  
   Do not change any settings. Make note of your machine's IP address for later.

5. **Proxy Configuration**  
   Skip this step by selecting "Done" and pressing `Enter`.

6. **Configure Ubuntu Mirror**  
   - For users in the US: Skip this step and select "Done."  
   - For users outside the US: Find your local Ubuntu mirror [here](https://launchpad.net/ubuntu/+archivemirrors) and replace the default one.

7. **Configure Storage**  
   - To use the entire disk: Select "Done."  
   - For custom partitions: Choose "Custom storage layout" and create the necessary partitions.

8. **Storage Summary**  
   Review the partition summary and select "Done."

9. **Confirm Partitioning**  
   Confirm the partitioning and select "Continue."

10. **Configure Profile**  
    Fill out the form with your name, server name, and user account settings (username and password).

11. **Configure SSH**  
    - Select OpenSSH.  
    - Import an SSH key for production servers (optional).  
    - For testing purposes, this step can be skipped.

12. **Install Featured Server Snaps**  
    Skip this by selecting "Done."

13. **Complete Installation**  
    Ubuntu will install. Once complete, select "Reboot."

---

## FiveM Setup and Configuration with Auto Updates

To skip auto-update configuration, proceed to the **Regular Installation** section.

### Step 1: Log In to Your Server via SSH
For beginners, I recommend **PuTTY**. You can use other SSH clients if preferred.

#### PuTTY Installation and Setup
1. Download PuTTY from [here](https://www.putty.org/).
2. Install PuTTY.
3. Open PuTTY.
4. In the **Hostname** box, enter your server's IP address. (Optional: Save the server for later by entering a name in the "Saved Sessions" box and clicking "Save.")
5. Click "Open."
6. Accept the SSH fingerprint by clicking "Yes."
7. Log in using the username and password you created during setup.

### Step 2: Update Server Repositories and Software
Run the following command:
```bash
sudo apt update && sudo apt upgrade -y
