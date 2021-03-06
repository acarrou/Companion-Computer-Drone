# PX4 Companion-Computer-Drone
## Introduction
This guide is meant to help people interested in making a drone using a Pixhawk as the flight controller with an onboard computer of your choice using Dronekit or ROS. This guide shows you how to use Dronekit using a Raspberry Pi and ROS with a Jetson Nano. However, they can both be done the same way with some changes. There are specific components that are needed to complete this project.


# Table of Contents
- [:one: Getting Started](#one-getting-started)
    - [Parts Required](#parts-required)
    - [Drone Assembly](#drone-assembly)
    - [Firmware](#firmware)
- [:two: The Onboard Computer](#two-the-onboard-computer)
    - [Jetson Nano (***THIS SECTION IS STILL UNDER CONSTRUCTION USE THE RASPBERRY PI FOR NOW***)](#jetson-nano)
        - [ROS Melodic Installation](#ros-melodic-installation)
        - [MAVROS Installation](#mavros-installation)
        - [Pixhawk and Jetson Nano Configuration](#pixhawk-and-jetson-nano-configuration)
        - [Jetson Nano and Pixhawk Communication](#jetson-nano-and-pixhawk-communication)
    - [Raspberry Pi](#raspberry-pi)
        - [GPIO Pin Configuration](#gpio-pin-configuration)
        - [Raspberry Pi Serial Configuration](#raspberry-pi-serial-configuration)
        - [Raspberry Pi and Pixhawk Communication](#raspberry-pi-and-pixhawk-communication)
        - [Dronekit](#dronekit)
            - [Simulation](#simulation)
            - [Running Dronekit on the Pi](#running-dronekit-on-the-pi)
            
<br><br/>
    

# :one: Getting Started
## Parts Required
Everything on the ***[Parts List](https://docs.google.com/document/d/1yg1S2lEn6Pxzbmr_OziczwqIsVRe4be1YQAyOio6FKI/edit?usp=sharing)*** is required for this project. Please read carefully what is needed. This is also assuming you have the necessary tools of a soldering iron, snips, and other essential tools needed for electronics.

## Drone Assembly
***[Here](https://youtube.com/playlist?list=PLm_39In9-wKMa8U90cwcDMecCGuM5Sja1)*** is a playlist of videos showing you how to build your drone. This will also go over how to flash the Firmware to the pixhawk. I will go more in-depth regarding the Firmware in the next area. You can choose either Ardupilot or PX4. However, I recommend PX4 Firmware.

Make sure your motors are in the correct configuration. You do this by making sure the motor wires are connected to the correct wires of the ESC. Below is a diagram of the direction of the motors and ESC configuration.
***I recommend you put on the propellers at the end. Not until you are ready to fly.***
<br><br/>
![alt text](https://ardupilot.org/copter/_images/motororder-quad-x-2d.png)
<br><br/>
![alt text](https://ardupilot.org/copter/_images/MOTORS_CW_CCWLegend.jpg)
<br><br/>
![alt text](https://aws.robu.in/wp-content/uploads/2019/03/connection.jpg)


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
We are using [QGroundControl](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html), it works great and makes it easy to set parameters. Follow this [video](https://www.youtube.com/watch?v=BNzeVGD8IZI&t=677s) on how to install the PX4 Firmware. If you would like to use the Ardupilot firmware, click on Ardupilot in the QGroundControl options. If you have issues with the latest px4 Firmware, you can look at earlier versions [here](https://github.com/PX4/PX4-Autopilot/releases), we are currently using ***v1.10.1***.
![alt text](https://docs.qgroundcontrol.com/master/assets/setup/firmware/firmware_select_default_px4.jpg)

<br><br/>

# :two: The Onboard Computer
There are two companion computers we recommend using the Jetson Nano or the Raspberry Pi. The Jetson Nano is a pricier embedded system compared to the Raspberry Pi. However, it makes an excellent system for object detection/avoidance. Make sure you have all the necessary parts to power and put an OS on these computers. You will need a device to put the OS on the micro sd card. Something like this -> ***[Micro SD Card USB Reader.](https://www.amazon.com/UGREEN-Reader-Portable-Adapter-Windows/dp/B0779V61XB/ref=sr_1_7?dchild=1&keywords=micro+sd+usb&qid=1609284957&sr=8-7)*** This makes it easy to image.
<br/>
[nKuylen](https://github.com/nKuylen) has made a platform in the back of the drone for the Raspberry pi or Jetson Nano. We recommend printing this or developing your own.

## Jetson Nano (***THIS SECTION IS STILL UNDER CONSTRUCTION USE THE RASPBERRY PI FOR NOW***)

We will be using ROS with the Jetson Nano. If you are unfamiliar with ROS, I recommend you learn it first through the [documentation](http://wiki.ros.org/Documentation) as this part will assume you know how to operate the catkin workspace. For more information on the Jetson nano, please follow this ***[site](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)***. It would be best to use ***[Balena Etcher](https://www.balena.io/etcher/)*** for an easy imaging process. A helpful video I found is ***[here](https://www.youtube.com/watch?v=fepv1uDyiXk)***. Use the image given for the Jetson Nano ***[here](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)***. Once you have successfully imaged your Jetson Nano put the sd card into the Nano.
### ROS Melodic Installation
We are using ROS Melodic as the Jetson Nano is using their NVIDIA® Jetson Nano™ Developer Kit deriving from Ubuntu 18.04. This ROS install is taken from [here](http://wiki.ros.org/melodic/Installation/Ubuntu)
<br/>

Setting your sources.list
<br/>
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
<br/>

Keys Setup:
<br/>
```bash
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
<br/>

Install:
<br/>
```bash
sudo apt update
sudo apt install ros-melodic-desktop-full
```
<br/>

Environment Setup:
<br/>
```bash
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
<br/>

(Optional) Dependencies for Building Packages:
<br/>
```bash
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```
<br/>

Install rosdep:
<br/>
```bash
sudo apt install python-rosdep
```

<br/>
Initialize rosdep:
<br/>

```bash
sudo rosdep init
rosdep update
```
<br><br/>

### MAVROS Installation
***--NEED TO WORK ON THIS--*** <br/>
MAVROS is the MAVLink extendable communication node for ROS with proxy for Ground Control Station.
<br><br/>
Install MAVROS:
<br/>
```bash
sudo apt-get install ros-melodic-mavros ros-melodic-mavros-extras
```
<br><br/>
Install Geographic datasets:
<br/>
```bash
wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
chmod a+x install_geographiclib_datasets.sh
sudo ./install_geographiclib_datasets.sh
```
<br><br/>

### Pixhawk and Jetson Nano Configuration
The configuration for the Pixhawk and Jetson Nano is straightforward. Use a micro USB to USB cable. Connect the micro USB side into the Pixhawk's serial port and the USB into the Jetson nano.
<br/>
![alt text](https://ardupilot.org/copter/_images/pixhawk_usb_connection.jpg)
<br><br/>

### Jetson Nano and Pixhawk Communication
After plugging the cord into the Pixhawk and Nano, you can run this command to see if everything is correct:
<br/>
```bash
roslaunch mavros px4.launch
```
<br/>

If there is a permissions issue with the serial port, make sure your user is in the 'dialout' group. You can do this by using this command (replace the 'enter username' with your username):
<br/>
```bash
sudo usermod -a -G dialout enter username
```
Congrats! You have successfully connected your Nano to your Pixhawk. If you are having trouble, this [video](https://www.youtube.com/watch?v=Irko6xb2qjs) may help you.
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
```bash
sudo raspi-config
```
And in the utility, select “Interfacing Options”:
<br/>
![alt text](https://ardupilot.org/dev/_images/RaspberryPi_Serial1.png)
<br><br/>
RasPiConfiguration Utility
And then “Serial”:
<br/>
![alt text](https://ardupilot.org/dev/_images/RaspberryPi_Serial2.png)
<br><br/>
When prompted, select ```no``` to “Would you like a login shell to be accessible over serial?”.

When prompted, select ```yes``` to “Would you like the serial port hardware to be enabled?”.

Reboot the Raspberry Pi when you are done.

The Raspberry Pi’s serial port will now be usable on ```/dev/serial0```.

<br><br/>


### Raspberry Pi and Pixhawk Communication

Once you have configured your Pi, you now have to change a few parameters on your pixhawk in QGroundControl or another ground control software. To set up the default companion computer message stream on TELEM 2, set the following parameters:
```bash
MAV_1_CONFIG = TELEM 2 (MAV_1_CONFIG is often used to map the TELEM 2 port)
MAV_1_MODE = Onboard
SER_TEL2_BAUD = 921600 (921600 or higher recommended for applications like log streaming or FastRTPS)
```

After you have changed those parameters, restart the Pixhawk. If not done already, connect the Raspberry Pi to the Pixhawk. You will need to install MAVProxy dependencies. You can find the MAVProxy install here -> [MAVProxy INSTALL GUIDE](https://ardupilot.org/mavproxy/docs/getting_started/download_and_installation.html#mavproxy-downloadinstalllinux). I will show below how to install these dependencies for Python 2 since that is what the Pi has on default.

For Python 2 on Debian based systems (including Ubuntu, WSL, Raspian):
<br/>
```bash 
sudo apt-get install python-dev python-opencv python-wxgtk4.0 python-pip python-matplotlib python-lxml python-pygame
pip install PyYAML mavproxy --user
echo "export PATH=$PATH:$HOME/.local/bin" >> ~/.bashrc
```

If you get a “permission denied” error message when connecting to serial devices, the user permissions may need to be changed:
<br/>
```bash 
sudo usermod -a -G dialout <username>
```

Make sure you update:
<br/>
```bash
pip install mavproxy --user --upgrade
```
Once finished installing everything, make sure to reboot the Pi.
<br><br/>


Next, run this command in the terminal to test the serial connection between the Pi and Pixhawk. If you are only getting the ```MAV>``` sign, make sure your parameters are correct for baud rate and correct telemetry port. It could also be your gpio pins are incorrect or not fully pushed on the RPI.
<br/>
Enter this command to test:
<br/>
```bash
mavproxy.py --master=/dev/serial0 --baudrate 921600 --aircraft MyCopter
``` 
You should see something like this:
<br/>
![alt text](https://ardupilot.org/dev/_images/RaspberryPi_ArmTestThroughPutty.png)
<br/>
You can also arm and test the throttle by typing :
<br/>
***MAKE SURE YOUR PROPS ARE OFF BEFORE!*** 
```bash
arm throttle
```

Then check to see if the pi can communicate with a computer using QGroundControl (Your Computer's IP running QGC)
```bash
mavproxy.py --master=/dev/serial0 --baudrate 921600 --out <Computer IP>:14550 --aircraft MyCopter 
```
<br><br/>

### Dronekit
#### **Simulation**
If simulating the drone using Dronekit to test scripts is something you're interested in following along. Otherwise, proceed to the Dronekit Installation on the Pi. We are using Ubuntu 18.04, but any other Ubuntu version should work.
<br><br/>

Clone the PX4 source code:
<br/>
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```
<br/>

Enter the directory:
<br/>

```bash
cd PX4-Autopilot/
```
<br/>

Install:
<br/>
```bash
bash ./Tools/setup/ubuntu.sh
```
<br/>

Check to see if you can run the Simulation on your Linux machine.
<br/>
Enter the directory:
<br/>

```bash
cd PX4-Autopilot/
```
<br/>

Choose from either Gazebo or jMAVSim:
<br/>
```bash
make px4_sitl_default jmavsim
```
OR 
<br/>

```bash
make px4_sitl gazebo
```
<br/>

In your Linux terminal, clone the Dronekit repository using:
<br/>
```bash
git clone https://github.com/dronekit/dronekit-python.git
```
<br/>
Enter the directory:
<br/>

```bash
cd ./dronekit-python
```
<br/>
Then build and install:
<br/>

```bash
sudo python setup.py build
sudo python setup.py install
```

While the Simulation is running in the background, you can use an IDE of your choice (We are using Pycharm). In the IDE, go to the directory and use the interpreter from the dronekit repository. Then run this code below that uses the Dronekit library.
<br/>
[Dronekit Mission Simulation Example](https://github.com/acarrou/Companion-Computer-Drone/blob/main/Dronekit-Scripts/DronekitMissionSimEx.py)
<br/>
Notice the code uses the IP - '127.0.0.1:14540' as this is the simulation's IP. Run the code and see it take off!
<br><br/>

#### **Running Dronekit on the Pi**
In your Raspberry Pi's terminal, enter these commands:

Update Pi
```bash
sudo apt-get update
sudo apt-get upgrade
```
Install Dronekit, and Dependencies
```bash
sudo apt-get install python-pip python-dev
sudo pip install pyserial
sudo pip install dronekit
sudo pip install MAVProxy
```
<br/>

Create a folder where you can put your dronekit scripts. Take the code from [Dronekit Mission Real Example](https://github.com/acarrou/Companion-Computer-Drone/blob/main/Dronekit-Scripts/DronekitMissionRealEx.py). Notice the connection string is different from the IP we had before in the Simulation. We also had to specify the baud rate in the vehicle initialization on line 40 for our pixhawk and Pi to communicate. Now that we have the code set up, lets run the program!
<br><br/>

Run Command:
<br/>
***ATTENTION! THIS PROGRAM ARMS THE DRONE AND EXECUTES THE PATH***
<br/>
```bash
python DronekitMissionRealEx.py --connect /dev/serial0
```
<br/>

You now have a drone running with Dronekit! For more information on Dronekit, visit their documentation page [here](https://dronekit-python.readthedocs.io/en/latest/about/index.html).






