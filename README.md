# Jetson_nano
Nvidia Jetson nano development board informations.

# Start with Jetson nano board
## Power the board
For power the board there are two way :
 1. Power with micro usb connector with the power limitation 5V/2A. In this case you can't have a lot of peripheral because 
 with 2A under 5V there are not enough power supply.
 2. Power with Barrel Jack 5V/4A. To use this power supply is **important to connect the jumper J48**.

![Top view board Jetson nano ](https://github.com/sulpub/Jetson_nano/blob/master/images/Jetson_nano_top_board_view.JPG)

# Getting Started With Jetson Nano Developer Kit

There are 4 stages for start using the dev board :
1. Power supply choice (micro usb or jack power). For jack power it's necessary to buy an external power supply (5V/4A)
2. Download the ISO file and install it on an SD card.
3. Setup the first boot with SD card, keyboard, mouse and ethernet connexions.
4. Test Jetson nano environment.

Note : For my board version, the lastest file not work. The correct file is the jetson-nano-sd-r32.1-2019-03-18.zip.

Link ISO jetson nano r32.1-2019-03-18 : (https://nvidia.box.com/shared/static/dp7xma0f4jbvttha6xo14km14fztal7o.zip)

# Enable and configure WIFI connexion

## wifi package

Add the wifi package with wpa

**sudo apt-get install wpasupplicant wireless-tools**

## wifi informations

For testing if wifi card is blocked

**sudo rfkill list**

For unblock all wifi board :

**sudo rfkill unblock all**

For obtain list of network in console mode :

**iw dev**

For list the visible wifi network on wlan0 interface

**sudo iw dev wlan0 scan**

For see the status of the wifi connexion

**sudo iw dev wlan0 link**

For statistic infirmations on the acces point :

**iw dev wlan0 station dump**

## wifi configuration

Edit interface file as well:

**sudo nano /etc/network/interfaces**

Enter this part :
```
auto wlan0
iface wlan0 inet static
       address 192.168.0.19
       netmask 255.255.255.0
       gateway 192.168.0.254
       wpa-ssid "network-name"
       wpa-psk "pre-shared-key"
```

Restart the network for testing

**sudo /etc/init.d/networking restart**

Link for information : https://linuxconfig.org/setup-wireless-interface-with-wpa-and-wpa2-on-ubuntu

Link for information : https://doc.ubuntu-fr.org/wifi

# Enable desktop sharing of embedded UBUNTU

By default there are somes problems to enable desktop sharing.

To activate it, it nesessary to do this
1. Edit the Xml file like : **sudo nano /usr/share/glib-2.0/schemas/org.gnome.Vino.gschema.xml**
2. Add this part on the XML file :
```
<key name='enabled' type='b'>
   <summary>Enable remote access to the desktop</summary>
   <description>
   If true, allows remote access to the desktop via the RFB
   protocol. Users on remote machines may then connect to the
   desktop using a VNC viewer.
   </description>
   <default>false</default>
</key>
```
3. Compile the Gnome schema with : **sudo glib-compile-schemas /usr/share/glib-2.0/schemas**
4. After this, you can enable desktop sharing.

![Jetson enable desktop sharing](https://github.com/sulpub/Jetson_nano/blob/master/images/jetson_nano_desktop_sharing.png)

Source information : http://bit.ly/2onnLIc

# deactivate GUI on boot
To disable GUI on boot, run:
```
    sudo systemctl set-default multi-user.target
```
To enable GUI again issue the command:
```
    sudo systemctl set-default graphical.target
```
to start Gui session on a system without a current GUI just execute:
```
    sudo systemctl start gdm3.service
``` 

# Install jupyter lab

    sudo apt install nodejs npm
    sudo apt install python3-pip
    sudo pip3 install jupyter jupyterlab
    sudo jupyter labextension install @jupyter-widgets/jupyterlab-manager
    sudo jupyter labextension install @jupyterlab/statusbar
    jupyter lab --generate-config
    sudo jupyter --build
    jupyter notebook password   

Source : https://github.com/NVIDIA-AI-IOT/jetbot/wiki/Create-SD-Card-Image-From-Scratch

# Run jupyter lab

    jupyter notebook --ip=0.0.0.0   //for running jupyter
    link to start on Jetson Nano : http://192.168.55.1:8888/lab? or http://127.0.0.1:8888/lab?
    
# Tensorflow on Jetson nano
Nvidia official TensorFlow release for Jetson Nano! (Python3.6+JetPack4.2.1)

    $ sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev
    $ sudo apt-get install python3-pip
    $ sudo pip3 install -U pip
    $ sudo pip3 install -U numpy grpcio absl-py py-cpuinfo psutil portpicker six mock requests gast h5py astor termcolor protobuf keras-applications keras-preprocessing wrapt google-pasta
    $ sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v42 tensorflow-gpu==1.14.0+nv19.7

DL frameworks - https://developer.nvidia.com/deep-learning-frameworks

Download - https://developer.qa.nvidia.com/embedded/downloads#?search=tensorflow

Installation guide - https://docs.nvidia.com/deeplearning/frameworks/install-tf-jetson-platform/index.html

Release note - https://docs.nvidia.com/deeplearning/dgx/install-tf-xavier-release-notes/index.html

## JetCard

JetCard is a system configuration that makes it easy to get started with AI. It comes pre-loaded with

    A Jupyter Lab server that starts on boot for easy web programming

    A script to display the Jetson's IP address (and other stats)

    The popular deep learning frameworks PyTorch and TensorFlow

After configuring your system using JetCard, you can get started prototyping AI projects from your web browser in Python.

See also

    **JetBot** - An educational AI robot based on NVIDIA Jetson Nano

    **JetRacer** - An educational AI racecar using NVIDIA Jetson Nano

    **JetCam** - An easy to use Python camera interface for NVIDIA Jetson

    **torch2trt** - An easy to use PyTorch to TensorRT converter

# utils

## Temperature sensor

To monitor temperature sensor

**sudo apt install lm-sensor**

after to know the temperature 

**sensors**

**tegrastats**

# NODE RED

## intallation of NODE RED


To install NODE RED run these command :
```
sudo apt-get install nodered
```

## Autostart on boot

If you want Node-RED to run when the Pi is turned on, or re-booted, you can enable the service to autostart by running the command:
```
sudo systemctl enable nodered.service
```
To disable the service, run the command:
```
sudo systemctl disable nodered.service
```

# INFLUXDB

![Influxdb database](https://github.com/sulpub/Jetson_nano/blob/master/images/influxdb.JPG)

## installation Influxdb

Package installation : 
```
sudo apt install influxdb
```
Or for manual installation, to install influxdb on Jetson you can go to the web site : https://portal.influxdata.com/downloads/

Run these command for ARM 64 bit
```
wget https://dl.influxdata.com/influxdb/releases/influxdb_2.0.0-alpha.18_linux_arm64.tar.gz
tar xvfz influxdb_2.0.0-alpha.18_linux_arm64.tar.gz
```

Get started with influxdb : https://v2.docs.influxdata.com/v2.0/get-started/

## add data on the database

For adding new database run influxdb in command line as well
```
 influx -port 8090
```
You can see some command for create a database
```
CREATE DATABASE base_name

SHOW DATABASES  //to see database name
```

You can manage the retention pilicy with these commands :
```
CREATE RETENTION POLICY "one_year" ON base_name DURATION 365d REPLICATION 1 DEFAULT //for database retention

RETENTION POLICY "one_year" ON nom_base DURATION 365d REPLICATION 1 DEFAULT        //for create retention policy Ex:1year=365days

ALTER RETENTION POLICY "one_year" ON "nom_base" DURATION 24h REPLICATION 1  //replace policy retention ex:1year->24h

DROP RETENTION POLICY "one_year" ON "nom_base" //delete retention policy

SHOW RETENTION POLICIES ON base_name

```

# GRAFANA

![Grafana visualisation](https://github.com/sulpub/Jetson_nano/blob/master/images/grafana.JPG)

## Grafana installation

To install Grafana go to this link : https://grafana.com/grafana/download?platform=arm

run these command :
```
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_5.2.0-beta1_arm64.deb
sudo dpkg -i grafana_5.2.0-beta1_arm64.deb
```

# INSTALL APACHE with php

Use this command to install apache

```
sudo apt install apache2
```

Use this command to install PHP

```
sudo apt install php php-mbstring
```

Use this command to install MySQL avec php

```
sudo apt install mysql-server php-mysql
```

Change the file property with this command on the directory /var/www/html

```
cd /var/www/html
sudo chown -Rf www-data:www-data .
sudo chmod -Rf 777 .
```

Run the web server with this command

```
sudo service apache2 start
```

For managing the mysql data base you can install the PhpMyAdmin tool with this command

```
//for install phpmyadmin
sudo apt install phpmyadmin
//validate all information and set a password for acces to the mysql

//for create a link to the phpmyadmin directory
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

```
