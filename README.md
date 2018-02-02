# rpiSetupHistory
Record of Raspberry PI Setup Procedure for ENGN1931Z

 
```
   .~~.   .~~.
  '. \ ' ' / .'
   .~ .~~~..~.
  : .~.'~'.~. :
 ~ (   ) (   ) ~
( : '~'.~.'~' : )
 ~ .~ (   ) ~. ~
  (  : '~' :  ) Raspberry Pi
   '~ .~~~. ~'
       '~'
```

(**NOTE**: the comptuer lab at B&H 191 has all the hardware you need to setup the RPi (keyboard, mouse, monitor). All computers on the East side have them. Connect, and press the button on the back of the monitor.)

## Setting up the basics

**This setup log will begin with quite explicit instructions, but as we proceed, we know that you will be able to help fill in the blanks.**

1. Begin with micro SD card loaded with latest NOOBS - New Out of the Box Software 

 + The microSDs provided with the class kit have been loaded with NOOBS v2.4.5. The microSDs were given to you inside SD adapters that may come in handy if your computer has a standard SD slot.
 + If you didn't get the class kit, you can follow the [instructions here](https://www.raspberrypi.org/documentation/installation/noobs.md) to download and load NOOBS on your own microSD.
 
2. Insert microSD into RPi, connect display+keyboard+mouse, and then power on.

 + Note that the RPi autodetects the display resolution on startup, so make sure to connect display first. (Otherwise, you will get very low resolution by default)

3. Select and Install Raspbian from NOOBS menu:

 + This step will take a while (~10-20 minutes depending on your microSD card speed), and after the installation is complete it will reboot to a graphic interface.

**(DO NOT connect to Wi-Fi, yet)**

4. Setup keyboard layout (special characters might not be correctly matched), location, and others by using going to the Raspberry icon in the upper left: `Menu` > `Preferences` > `Raspberry Pi Configuration` : `Localization`

5. Finding MAC address of the Raspberry Pi and registering it to connect seamlessly to the Brown-Guest WiFi network:

  + Find your MAC (`HWaddr`) address of your WiFi adapter using the `ifconfig` command. Launch the terminal (using the command prompt icon in the upper left), type `ifconfig` and press Enter. In the block under `wlan0` look for the set of characters after `ether` (e.g. b8:27:eb:51:22:21).

  + USING ANOTHER COMPUTER. Register your device at http://guestwifi.net.brown.edu/. This helps the Raspberry Pi connect automatically to Wi-Fi without user input after the first successful connection.

6. Back to the Raspberry Pi. Connect to the internet using WiFi:

  + Select Brown-Guest using the network icon in upper right.

  + Launch Chromium web browser using the world icon in the upper left.

  + Test your connection. **(Note that you should NOT accept Brown Guest WiFi Terms; if you see a terms window, please cancel,and repeat step 5.)**
  
  + In a terminal run the `ifconfig` command again, this time note and record your IP address for future use. It is found in the `wlan0` section of the `ifconfig` output, right after `inet` (e.g. 172.18.xx.xx). In the future it might occurr that your Raspberry Pi connects correctly to Brown-Guest but that its IP address is changed, one of several way getting this new IP address, withouth having to log in to the Pi or hooking up a monitor and keyboard, is going to the website used to register the MAC address http://guestwifi.net.brown.edu/ : Manage Devices : Click on your Pi info : Click on Print Icon. IP address should show here.
  
  + Please complete the ENGN1931Z RPi Information form here: http://goo.gl/rn4nHT

7. Launch a terminal (black icon in top left) and upgrade to the latest Raspbian distribution using following commands (note that this may take 15-20 minutes depending on the WiFi speed):

   ```
   sudo apt-get update
   sudo apt-get dist-upgrade
   ```

8. Change Login Password using the terminal command `passwd`. Note that the default username is `pi` and the default password is `raspberry` for all RPis.

9. Generate new Secure Shell (SSH) keys using the following terminal command:

   ```
   sudo rm /etc/ssh/ssh_host_* && sudo dpkg-reconfigure openssh-server
   ```

10. Enable SSH on the Raspberry Pi, and set up your personal computer (client) to use it  [following the platform dependent instructions found here](https://www.raspberrypi.org/documentation/remote-access/ssh/):

 - **Now you should be able to SSH into your RPi** using the IP address you recorded earlier (Step 6) together with the username `pi` and your password (Step 8).

11. Setup basic (**unencrypted**) VNC - Virtual Network Connection [following subset of instructions here](https://www.raspberrypi.org/documentation/remote-access/vnc/)

   + Install and launch tightvnc on RPi from terminal: 
   
```
sudo apt-get install realvnc-vnc-server realvnc-vnc-viewer
vncserver  :1 -geometry 1920x1080 -depth 24
```
**When setting up tightvnc, choose a password that is distinct from your RPi login.**

  + Install and launch Real VNC's Chrome App [link here](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla) on your personal computer.
  + Connect to your RPi by entering IP address (from Step 9) followed by `:` and the port number for tightvnc (usually `1`).
    - For example, if IP address is `172.18.24.24` and the vnc is on port `1`, then you should use `172.18.24.24:1`
    - Login with your vnc password (see step 11 above).
    
12. To have the above run every time the RPi boots open up a terminal and do the following:

```
cd .config
mkdir autostart
cd autostart
nano tightvnc.desktop
```

The last line creates the file `tightvnc.desktop`. Nano is a commandline utility for text editing. Fill it up like so:

```
[Desktop Entry]
Type=Application
Name=TightVNC
Exec=vncserver :1 -geometry 1920x1080 -depth 24
StartupNotify=False
```

After putting file contents, exit and save file by doing: `Ctrl+x`, `y`.
