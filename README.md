# rpiSetupHistory
Record of Raspberry PI Setup Procedure for ENGN1931Z

## Setting up the basics

**This setup log will begin with quite explicit instructions, but as we proceed, I assume that you will be able to help fill in the blanks.**

1. Begin with micro SD card loaded with latest NOOBS - New Out of the Box Software 

 + The microSDs provided with the class kit have been loaded with NOOBS v2.4.5.
 + Alternatively, you can follow the [instructions here](https://www.raspberrypi.org/documentation/installation/noobs.md) to download and load NOOBS on your own microSD.
 
2. Insert microSD into RPi, connect display+keyboard+mouse, and then power on.

 + Note that the RPi autodetects the display resolution on startup, so make sure to connect display first. (Otherwise, you will get very low resolution by default)

3. Select and Install Raspbian from NOOBS menu

 + This step will take a while (~10-15 minutes), and after the installation is complete it will reboot to a graphic interface.

4. Setup keyboard layout (special characters might not be correctly matched), location, and others by using going to `Menu` > `Preferences` > `Raspberry Pi Configuration` : `Localization`

5. Connect to the internet  via ethernet cable or Brown-Guest WiFi (instructions below).

  + Select Brown-Guest using network icon in upper right.

  + Launch Chromium web browser using the world icon in the upper left.

  + Read and accept the Brown-Guest WiFi login terms. Test your connection.
 
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

6. Launch a terminal (black icon in top left) and upgrade to the latest Raspbian distribution using following commands:

   ```
   sudo apt-get update
   sudo apt-get dist-upgrade
   ```

7. Change Login Password using the terminal command `passwd`. Note that the default username is `pi` and the default password is `raspberry` for all RPis.

8. Generate new Secure Shell (SSH) keys using the following terminal command:

   ```
   sudo rm /etc/ssh/ssh_host_* && sudo dpkg-reconfigure openssh-server
   ```

9. Register your RPi with Brown to bypass WiFi terms acceptance in the future, and use IP address for VNC login:

  + Find your MAC (`HWaddr`) address of your WiFi adapter by using `ifconfig` command. Under the block under `wlan0` look for the set of characters after `ether`(e.g. b8:27:eb:51:22:21).

  + Note and record your IP address for future use. It is also found in the `wlan0` section of the `ifconfig` output, right after `inet` (e.g. 172.18.xx.xx).

  + Register your device at http://guestwifi.net.brown.edu/. This helps the Raspberry Pi connect automatically to Wi-Fi without user input.

10. Enable SSH on the Raspberry Pi, and set up your personal computer (client) to use it  [following the platform dependent instructions found here](https://www.raspberrypi.org/documentation/remote-access/ssh/).

 - **Now you should be able to SSH into your RPi** using the IP address you recorded earlier (Step 9) together with the username `pi` and your password (Step 7).

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

11. Setup basic (**unencrypted**) VNC - Virtual Network Connection [following subset of instructions here](https://www.raspberrypi.org/documentation/remote-access/vnc/)

   + Install and launch tightvnc on RPi from terminal: 
   
```
sudo apt-get install tightvncserver
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

## Setting up Python3 and Jupyter Notebooks

```
sudo apt-get install python3-matplotlib
sudo apt-get install python3-scipy
pip3 install --upgrade pip
sudo pip3 install jupyter
```

Run notebook interface using:

```
jupyter notebook
```

## Installing pyQT4 for Python3

pyQT allows us to create simple GUIs using Python. Set it up like so:


```
sudo apt-get install qt4-default pyqt4-dev-tools
sudo apt-get install python3-pyqt4
```

## Using Selenium in Raspberry Pi with Firefox

Selenium allows us to programmatically control a browser. Set it up like so:

1. Follow instructions in [https://mozilla.debian.net](https://mozilla.debian.net) to install Firefox in Raspian. Choose options so that the options chosen read: I'm running Debian **(oldstable (Jessie))** and I want to install **Firefox** version **esr52**.

+ Download [geckodriver-0.18.0-arm7hf.tar.gz](https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-arm7hf.tar.gz).
+ Decompress it:

```
tar -xzf geckodriver-0.18.0-arm7hf.tar.gz
```
+ Move the extracted file `geckodriver` to `/usr/local/bin/` using admin privileges:

```
sudo mv geckodriver /usr/local/bin/
```

+ Use pip3 to install selenium:

```
sudo pip3 install selenium
```
