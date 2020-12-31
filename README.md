# Introduction
This guide is meant to help people interested in making a drone using a Pixhawk as the flight controller with an onboard computer of your choice using Dronekit or ROS. This guide shows you how to use Dronekit using a Raspberry Pi and ROS with a Jetson Nano. However, they can both be done the same way with some changes. There are specific components that are needed to complete this project.


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
        - [Raspberry Pi Serial Configuration](#raspberry-pi-serial-configuration)
        - [Raspberry Pi and Pixhawk Communication](#raspberry-pi-and-pixhawk-communication)
        - [Dronekit](#dronekit)
    

# :one: Getting Started
## Parts Required
Everything on the ***[Parts List](https://docs.google.com/document/d/1yg1S2lEn6Pxzbmr_OziczwqIsVRe4be1YQAyOio6FKI/edit?usp=sharing)*** is required for this project. Please read carefully what is needed. This is also assuming you have the necessary tools of a soldering iron, snips, and other essential tools needed for electronics.

## Drone Assembly
***[Here](https://youtube.com/playlist?list=PLm_39In9-wKMa8U90cwcDMecCGuM5Sja1)*** is a playlist of videos showing you how to build your drone. This will also go over how to flash the firmware to the pixhawk. I will go more in-depth regarding the firmware in the next area. You can choose either Ardupilot or PX4. However, I recommend PX4 Firmware.

Make sure your motors are in the correct configuration. You do this by making sure the motor wires are connect to the correct wires of the ESC. Below is a diagram of the direction of the motors and ESC configuration.
***I recommend you put on the propellers very last. Not until you are ready to fly.***
<br><br/>
![alt text](https://ardupilot.org/copter/_images/motororder-quad-x-2d.png)
<br><br/>
![alt text](https://ardupilot.org/copter/_images/MOTORS_CW_CCWLegend.jpg)
<br><br/>
![alt text](https://aws.robu.in/wp-content/uploads/2019/03/connection.jpg)




***---EDIT THIS (Add more info)---*** :arrow_down:
<br><br/>
Plug the ESC pins into back of the Pixhawk. (order should be from top-> -, s, +) ***Make sure they are in the correct order or your drone will fly incorrectly.***
<br><br/>
![alt text](https://ardupilot.org/copter/_images/Pixhwak_outputs.jpg)
<br><br/>
Plug in the rest of  the modules you've puchased into their respected slots as showed in this image:
<br><br/>
![alt text](https://ardupilot.org/copter/_images/Pixhawk-Inforgraphic2.jpg)

<br><br/>

## Firmware
We are using [QGroundControl](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html), it works great and makes it easy to set parameters. Follow this [video](https://www.youtube.com/watch?v=BNzeVGD8IZI&t=677s) on how to install the PX4 Firmware. If you would like to use the Ardupilot firmware click on Ardupilot in the QGroundControl options. If you have issues with the latest px4 firmware you can look at earlier versions [here](https://github.com/PX4/PX4-Autopilot/releases), we are currently using ***v1.10.1***.
![alt text](https://docs.qgroundcontrol.com/master/assets/setup/firmware/firmware_select_default_px4.jpg)

<br><br/>

# :two: The Onboard Computer
There are two companion computers we recommend using the Jetson Nano or the Raspberry Pi. Make sure you have all the necessary parts to power and put an OS on these computers. You will need a device to put the OS on the micro sd card. Something like this -> ***[Micro SD Card USB Reader.](https://www.amazon.com/UGREEN-Reader-Portable-Adapter-Windows/dp/B0779V61XB/ref=sr_1_7?dchild=1&keywords=micro+sd+usb&qid=1609284957&sr=8-7)*** This makes it easy to image. 

## Jetson Nano
For more information on the Jetson nano, please follow this ***[site](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)***. It would be best if you used ***[Balena Etcher](https://www.balena.io/etcher/)*** for an easy imaging process. A helpful video I found is ***[here](https://www.youtube.com/watch?v=fepv1uDyiXk)***. Use the image given for the Jetson Nano ***[here](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)***. Once you have successfully imaged your Jetson Nano put the sd card into the nano.
### ROS Melodic Installation
***--NEED TO DO THIS--***
<br><br/>
<br><br/>
### MAVROS
***--NEED TO DO THIS--***
<br><br/>
<br><br/>


## Raspberry Pi
Start by Installing the Raspberry Pi OS using Raspberry Pi Imager ***[here](https://www.raspberrypi.org/software/)***. Image the Pi with the recommended OS. You can use this ***[video](https://www.youtube.com/watch?v=y45hsd2AOpw)*** to help you image the Pi.

### GPIO Pin Configuration
In order for the Pi to communicate with the Pixhawk, we need to connect four cables. They are 5V, GND, TX, and RX. You will need to create the connector yourself. I purchased this ***[kit](https://www.amazon.com/1-25mm-Connectors-Pre-Crimped-Pixhawk-Silicone/dp/B07S18D3RN/ref=pd_lpo_421_img_1/141-3051472-7066820?_encoding=UTF8&pd_rd_i=B07S18D3RN&pd_rd_r=f3a9d5f4-62e9-4587-960a-f1adead929ca&pd_rd_w=1BF6V&pd_rd_wg=si57j&pf_rd_p=7b36d496-f366-4631-94d3-61b87b52511b&pf_rd_r=M89NX5SX5CSHB6VGESD6&psc=1&refRID=M89NX5SX5CSHB6VGESD6)*** to make my own. I then soldered the connectors from the pixhawk to 4 female jumper wires.

***Here is the configuration of the pins***
![alt text](https://discuss.ardupilot.org/uploads/default/original/2X/f/f837b6b1116ec02c3490e34035c2f09da5a62936.jpg)

<br><br/>

### Raspberry Pi Serial Configuration

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


### Raspberry Pi and Pixhawk Communication

Once you have configured you Pi, you now have to change a few parameters on your pixhawk in QGroundControl or another ground control software. To set up the default companion computer message stream on TELEM 2, set the following parameters:
```
MAV_1_CONFIG = TELEM 2 (MAV_1_CONFIG is often used to map the TELEM 2 port)
MAV_1_MODE = Onboard
SER_TEL2_BAUD = 921600 (921600 or higher recommended for applications like log streaming or FastRTPS)
```

After you have changed those parameters restart the Pixhawk. If not done already, connect the Raspberry Pi to the Pixhawk. Run this command in the terminal to test if the connection is made.
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



