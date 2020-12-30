# Table of Contents
- [Introduction](#introduction)
- [:one: Getting Started](#one-getting-started)
    - [Parts Required](#parts-required)
    - [Drone Assembly](#drone-assembly)
    - [Firmware](#firmware)
- [:two: The Onboard Computer](#two-the-onboard-computer)
    - [Jetson Nano](#jetson-nano)
        - [ROS Melodic Installation](#ros-melodic-installation)
        - [MAVROS](#mavros)
    - [Raspberry Pi](#raspberry-pi)
        - [GPIO Pin Configuration](#gpio-pin-configuration)
        - [Connection to the Pixhawk](#connection-to-the-pixhawk)
        - [Dronekit](#dronekit)
    

# Introduction
This guide is here to help people interested in making a drone using a Pixhawk to control it with an onboard computer of your choice using Dronekit or ROS. There are specific components that are needed in order to complete this project.

# :one: Getting Started
## Parts Required
Below is a link with the parts required for this project. Please read carefully on what is needed. This is also assuming you have the necessary tools of a soldering iron, snips and other basic tools needed in electronics. :arrow_down:
***[Parts List](https://docs.google.com/document/d/1yg1S2lEn6Pxzbmr_OziczwqIsVRe4be1YQAyOio6FKI/edit?usp=sharing)***
## Drone Assembly
***[Here](https://youtube.com/playlist?list=PLm_39In9-wKMa8U90cwcDMecCGuM5Sja1)*** is a playlist of videos showing you on how to build your drone. This will also go over how to flash the firmware to the pixhawk. <br/> <ins>I will go more in depth regarding the firmware in the next area.</ins> You can choose either Ardupilot or PX4, however I recommend PX4 Firmware.

|
(ENTER VIDEOS WE HAVE HERE)
## Firmware

# :two: The Onboard Computer
There are two companion computers we recommend using, the Jetson Nano or the Raspberry Pi. Make sure you have all necessary parts to power and put an OS on these computers. You will need a device to put the OS on the micro sd card. Something like this -> ***[Micro SD Card USB Reader.](https://www.amazon.com/UGREEN-Reader-Portable-Adapter-Windows/dp/B0779V61XB/ref=sr_1_7?dchild=1&keywords=micro+sd+usb&qid=1609284957&sr=8-7)*** This makes it easy to image. 

## Jetson Nano
For more information on the Jetson nano please follow this ***[site](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)***. You should use ***[Balena Etcher](https://www.balena.io/etcher/)*** for an easy imaging process. A helpful video I found is ***[here](https://www.youtube.com/watch?v=fepv1uDyiXk)***. Use the image given for the Jetson Nano ***[here](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)***. Once you have successfully imaged your Jetson Nano put the sd card into the nano.
### ROS Melodic Installation
***--NEED TO DO THIS--***
<br><br/>
<br><br/>
### MAVROS
***--NEED TO DO THIS--***
<br><br/>
<br><br/>


## Raspberry Pi
Start by Installing the Raspberry Pi OS using Raspberry Pi Imager ***[here](https://www.raspberrypi.org/software/)***. Image the Pi with the reccomended OS. You can use this ***[video](https://www.youtube.com/watch?v=y45hsd2AOpw)*** to help you image the Pi.

### GPIO Pin Configuration
In order for the Pi to communicate with the Pixhawk we need to connect four cables. They are 5V, GND, TX and RX. You will need to create the connector yourself. I purchased this ***[kit](https://www.amazon.com/1-25mm-Connectors-Pre-Crimped-Pixhawk-Silicone/dp/B07S18D3RN/ref=pd_lpo_421_img_1/141-3051472-7066820?_encoding=UTF8&pd_rd_i=B07S18D3RN&pd_rd_r=f3a9d5f4-62e9-4587-960a-f1adead929ca&pd_rd_w=1BF6V&pd_rd_wg=si57j&pf_rd_p=7b36d496-f366-4631-94d3-61b87b52511b&pf_rd_r=M89NX5SX5CSHB6VGESD6&psc=1&refRID=M89NX5SX5CSHB6VGESD6)*** to make my own. I then soldered the connectors from the pixhawk to 4 female jumper wires.

***Here is the configuration of the pins***
![alt text](https://discuss.ardupilot.org/uploads/default/original/2X/f/f837b6b1116ec02c3490e34035c2f09da5a62936.jpg)

<br><br/>
<br><br/>


### Connection to the Pixhawk

On your Raspberry Pi, enter:
```sudo raspi-config```
And in the utility, select “Interfacing Options”:
![alt text](https://ardupilot.org/dev/_images/RaspberryPi_Serial1.png)
<br><br/>
RasPiConfiguration Utility
And then “Serial”:
<br><br/>
![alt text](https://ardupilot.org/dev/_images/RaspberryPi_Serial2.png)
<br><br/>
When prompted, select ```no``` to “Would you like a login shell to be accessible over serial?”.

When prompted, select ```yes``` to “Would you like the serial port hardware to be enabled?”.

Reboot the Raspberry Pi when you are done.

The Raspberry Pi’s serial port will now be usable on ```/dev/serial0```.

<br><br/>
<br><br/>

Once you have configured you Pi, you now have to change a few parameters on your pixhawk in QGroundControl or another ground control software. To set up the default companion computer message stream on TELEM 2, set the following parameters:
```
MAV_1_CONFIG = TELEM 2 (MAV_1_CONFIG is often used to map the TELEM 2 port)
MAV_1_MODE = Onboard
SER_TEL2_BAUD = 921600 (921600 or higher recommended for applications like log streaming or FastRTPS)
```


Once you have changed those parameters you can run this command in the terminal to test if the connection is made.
```
mavproxy.py --master=/dev/serial0 --baudrate 921600 --aircraft MyCopter
``` 

Then check to see if the pi can communicate with a ncomputer using QGroundControl
```
mavproxy.py --master=/dev/serial0 --baudrate 921600 --out <Computer IP>:14550 --aircraft MyCopter 
```

### Dronekit
***--NEED TO DO THIS--***
<br><br/>
<br><br/>



