# rpiSetupHistory
Record of Raspberry PI Setup Procedure for ENGN1931Z

** This setup log will begin with quite explicit instructions, but as we proceed, I assume that you will be able to help fill in the blanks.

1. Begin with micro SD card loaded with latest NOOBS - New Out of the Box Software 
 - The microSDs provided with the class kit have been loaded with NOOBS v1.9 (Build-date: 2016-03-18)
 - Alternatively, you can follow the [instructions here](https://www.raspberrypi.org/documentation/installation/noobs.md) to download and load NOOBS on your own microSD.
 
2. Insert microSD into RPi, connect display+keyboard+mouse, and then power on.
 - Note that the RPi autodetects the display resolution on startup, so make sure to connect display first. (Otherwise, you will get very low resolution by default)

3. Select and Install Raspbian from NOOBS menr
 - This step will take a while (~10-15 minutes), and after the installation is complete it will reboot to a graphic interface.

4. Setup keyboard, location, etc. using top `Menu` > `Preferences` > `Rasp. Pi Config.`

5. Connect to the internet  via ethernet cable or Brown-Guest WiFi (instructions below).
  1. Select Brown-Guest using network icon in upper right.
  2. Launch Epiphany web browser using icon in upper left
  3. Navigate to any address to see and accept Brown-Guest WiFi login terms   

6. Launch a terminal and update to lastest Raspbian distribution using following commands:

   ```
   sudo apt-get update
   sudo apt-get dist-upgrade
   ```

7. Change Login Password using the terminal command `passwd`
 - Note that the default username is `pi` and the default password is `raspberry` for all RPis.

8. Generate new Secure Shell (SSH) keys using the following terminal command:

   ```
   sudo rm /etc/ssh/ssh_host_* && sudo dpkg-reconfigure openssh-server
   ```

9. Register your RPi with Brown to bypass WiFi terms acceptance in the future
  1. Find your MAC (`HWaddr`) address of your WiFi adapter (`wlan0`) by using `ifconfig` command
  2. Note and record your `IPv4 address` for the future use. (This should probably resemble  172.18.xx.xx)
  2. Register your device at http://guestwifi.net.brown.edu/ **using another computer**. (The web browser on RPi is not secure, so please do not use it to enter your Brown credentials.)

10. Setup SSH on your personal computer [following the platform dependent instructions found here](https://www.raspberrypi.org/documentation/remote-access/ssh/)
 - **Now you should be able to SSH into your RPi** using the IPv4 address you recorded earlier (Step 9) together with the username `pi` and your password (Step 7).

11. Setup basic (**unencrypted**) VNC - Virtual Network Connection [following subset of instructions here](https://www.raspberrypi.org/documentation/remote-access/vnc/)
   1. Install and launch tightvnc on RPi from terminal: 
   
      ```
      sudo apt-get install tightvncserver
      vncserver  :1 -geometry 1920x1080 -depth 24
     ```
  2. Install and launch Real VNC's Chrome Add-on [link here](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla) on your personal computer
  3. Connect to your RPi by entering IPv4 address (from Step 9) followed by `:` and the port number for tightvnc (usually `1`).
    - For example, if IPv4 address is `172.18.24.24` and the vnc is on port `1`, then you should use `172.18.24.24:1`
    - Login with your username (default `pi` unless you changed it) and your password (from Step 7)
