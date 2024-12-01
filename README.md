# FivemDebian
Custom FiveM Debian Install

VM Install Guide - If you already know how to install and launch a VM skip this
  Getting Started:
    Things you WILL need
    1. Ubuntu Server ISO File
        First download the arch iso file found here: https://ubuntu.com/download/server

    2. Virtual Box, Hyper V, or VM Ware

        You can pick any one of these options:
            VM Ware Install Guide: https://www.youtube.com/watch?v=PoNPBdKLZdk
            Hyper V Guide: https://www.youtube.com/watch?v=FCIA4YQHx9U
            Virtual Box Install Guide: https://www.youtube.com/watch?v=8mns5yqMfZk

    3. VM Instance
        Using one of the three guides create a vm instance with the Ubuntu Linux ISO
        I skipped the guide for creating a vm instance due to the quality of the guides provided

Ubuntu Install Guide
    1. Select language using up and down arrow and enter to select
    2. Update installer select Update the new installer and wait
    3. Choose your keyboard layout
    4. Configure Network don't change any of these settings just make note of your machines IP address we will need it
    5. Skip proxy configuration by making sure the done option is selected and pressing enter
    6. Configure ubuntu mirror if you're in the US you can skip this and select done if you're outside the us find your local ubuntu mirror and replace the default one. found here https://launchpad.net/ubuntu/+archivemirrors
    7. Configure storage if you plan to use the entire disk skip this step by selecting done otherwise select custom sorage layout and create the partitions you need.
    8. This next screen is a summary of the partitions ubuntu is creating just select done.
    9. Confirm the distructive action this is comfirming you want to partition the disk and install ubuntu select continue.
    10. Configure profile fill out the form with your name and prefered server name and, user account settigs such as username and password.
    11. Configure SSH make sure you select openssh you can import an ssh key if you know how for big production servers this is reccomended as it is an extra bit of security for testing purposes we wont be doing this.
    12. Skip install featured server snaps by going down to the done option and pressing enter as we dont need any of these extra packages.
    13. Ubuntu will start the install once done select reboot.

FiveM Setup and configuration
    1. login to your server via your prefered ssh client for beginners I recomend putty as it's simple to use if you prefer to use something else that is okay
        1. Download putty from here https://www.putty.org/
        2. Install Putty
        3. Open Putty
        4. In the hostname box enter your servers ip address - optional save the server by putting the the name for your server in the saved sections box and pressing save
        5. Press enter
        6. Hit accept this is just confirming the ssh fingerprint 
        7. Login using the username and password you made in the setup
    2. update your server repositories and software by running ths command:
        ```bash
        sudo apt update && sudo apt upgrade -y
        ```
    3. Install git and and nano xz-utils by running this command
        ```bash
        sudo apt install git nano xz-utils -y
        ```
    4. Install and configure MariaDB
        1. Install MariaDB using the following command:
        ```bash
        sudo apt install mariadb-server -y
        ```
        2. Start MariaDB using the following command - This is only done once as we have the service enabled to run on startup by default
        ```bash
            sudo systemctl start mariadb.service
        ```
        3. Configure MariaDB by running the following command:
        ```bash
        sudo mysql_secure_installation
        ```
        4. Enter your password
        5. Type n at the switch to unix authentication prompt then hit enter.
        6. At the change root password prompt type n and hit enter.
        7. For security reasons at the remove anonymous users prompt type a capital Y and hit enter.
        8. PLEASE FOR THE LOVE OF ALL THAT IS HOLY at the Dissallow root login remotely prompt type capital Y and hit enter.
        9. Optional Remove test database you can leave it there if you wold like but you don't really need it.
        10. At the reload privlisges table prompt type a capital Y and hit enter.
    5. Making a user and database for your server aswell as granting access to that user to change that database
        1. Open the mySQL command line by running the followig:
            ```bash
            sudo mysql
            ```
        2. Create user and assign a password using the following command - please change the defauld password from password123! to something more secure keep in mind yor devs will see this so don't make it your personal password
        ```bash
        create user 'fivem'@'%' identified by 'password123!';
        ```
        3. Modify user privileges and grand privileges to your fivem user on all your databases
        ```bash
        grant all privileges on *.* to 'fivem'@'%';
        ```
        4. Flush Privileges:
        ```bash
        flush privileges;
        ```
        5. Exit mySQL
        ```bash
        exit
        ```
        6. Restart mariaDB:
        ```bash
        sudo systemctl restart mariadb
        ```

    6. Make a directory for fx server by running the following command: 
        ```bash
        mkdir -p ~/FXServer/server
        ```
    7. Enter the directory by using the following command:
        ```bash
        cd ~/FXServer/server
        ```
    8. Download the current latest server version by running the following command
        1. Get the latest server version here https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/
        2. Right click the grey download icon for the latest version listecd and select copy
        3. Go over to your ssh terminal and type wget followed by a space and paste the link should look someting like this:
        ```bash
        wget https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/11535-5053bc6428bd16c6d1e6e8c4cab29afe0cc9bfa0/fx.tar.xz
        ```
        4. Then Extract the server file using the following:
        ```bash
            tar xf fx.tar.xz
        ```
        5. Remove the fx.tar.xz file as its not needed by running the following:
        ```bash
        rm fx.tar.xz
        ```
        6. Exit the server folder and enter the server-data folder using the following command:
        ```bash
        cd ../server-data
        ``` 
        7. Obtain a server licence key from here https://keymaster.fivem.net/login?return_url=%2Fasset-grants if you do not have a cfx.re fourm account create one after signing in click the New server option on the left side of the screen underneath the Server Owners Section. Name your server and verify the Captcha then click the blue Generate button keep note of your server key it should look something like this: cfxk_1ZUo7hdalh8HWKRYnuRkX_10GG9d

Starting your server 
    There are a few different options you can either run it using a program called Screen or by configuring a linux service both are good options I recomend configuring a linux service as it makes it simple to manage as you can make your server automaticly start when you turn on your vm instance make sure that you port forward your server using port 30120 tcp and udp.
        Screen instructions:
            For the first run you must use Screen or just run the run.sh script as we need the cfx.re pin provided in the ssh terminal
        Note: for a more comprehensive guide to Screen look here https://linuxize.com/post/how-to-use-linux-screen/ or here https://www.youtube.com/watch?v=_ZJiEX4rmN4&list=LL&index=5
            1. Installing Screen
            ```bash
            sudo apt install screen -y
            ```
            2. Run your server using Screen with the following commands:
            ```bash
            screen -S fivem
            ```
            ```bash
            cd ~/FXServer/ && bash ~/FXServer/server/run.sh
            ```

Configuring txAdmin
    1. Login to the tx admin panel by opening your web browser and typing in the local ip of your vm instance followed by the port example:
    10.0.0.231:40120
    2. link your cfx.re account with the pin provided in your ssh terminal
    3. create a backup password
    4. configure your server to your own preferences for a name
    5. Select your recipie 
    6. Select your template 
    7. change the server location to be 
    ```bash
    /home/yourusername/FXServer/server-data
    ```
    8. Go to recipie Developer
    9. click Next
    10. paste your server licence key
    11. click show/hide database options and enter your username and password you setup previously with mariaDB
    12. Run Recipie
    13. modify any other server config options you need to you can change them at anytime then hit save and run
    14. Celibrate!!! your server is on 

Configuring linux service optional
    1. Change directory to the following
    ```bash
    cd /etc/systemd/system
    ```
    2. Make a service file
    ```bash
    sudo nano fivem.service
    ```
    3. Copy and paste the following into your file then pres Ctrl+s and Ctrl+x to write and quit
    ```bash
    [Unit]
    Description=FiveM Server Service

    [Service]
    Type=simple
    ExecStart=/home/as1ch/FXServer/server/run.sh
    Restart=always

    [Install]
    WantedBy=default.target
    ```
    4. Enable the service using the following
    ```bash
    sudo systemctl enable fivem.service
    ```
    5. Start FiveM service using the following
    ```bash
    sudo systemctl start fivem.service
    6. If you ever shutdown your server in tx admin you will need to run the following to bring your server back up optional you can also reboot your vm instance
    ```bash
    sudo systemctl restart fivem.service
    ```    
