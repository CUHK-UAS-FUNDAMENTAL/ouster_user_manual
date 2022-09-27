# Ouster Lidar OS-0 Commission Manual

---

The aim of this tutorial is to tell the beginner how to rapidly deploy OUSTER lidar on your computer systems. Roughly, this tutorial can be divided into hardware connection and software configuration.

[TOC]

## Hardware Illustration 

First of all, the user should figure out the required equipments for this test. The hardware list is listed as follows.

| Name                                   | Number | Description                                                  |
| :------------------------------------- | :----- | ------------------------------------------------------------ |
| OS0-32                                 | 1      | Laser sensors                                                |
| Interface Box                          | 1      | Receive and code data from laser sensors, and power the OS0-32 |
| Ethernet Network Patch Cable           | 1      | Connect PC and interface box                                 |
| Electrical Wire Aviation Connector     | 1      | Connect interface box and OS0-32                             |
| 220 V to 24 V AC/DC Adapter            | 1      | Power the interface box                                      |
| Personal Computer (i.e., TX2, NX, NUC) | 1      | Decode sensor data and visualize it                          |

<img src="/run/user/1000/doc/be460002/20200902203207373.png" alt="Figure1: Hardware Connection" style="zoom:80%;" />

## IP Configuration

After the user finish the electrical connection of hardware equipment, the OS0-32 shall begin to work.  Then, Obvious vibration and heat can be felt by user. 

The wire network of PC shall be configured as follows:

<img src="/home/gzx/图片/2022-09-14 14-00-38 的屏幕截图.png" alt="2022-09-14 14-00-38 的屏幕截图" style="zoom:67%;" />

Then, restart the wire network.

For testing whether PC successfully connect to the sensor. Please open a terminal and type following command.

```bash
ping -c1 os-122233001531.local
```

Note: 122233001531 is the host name of this LIDAR which can be found from the top of OS0-32.

The user can use following website to see the state and hardware configuration of OS0-32.

```bash
http://os-122233001531.local/
```

### First Usage of LIDAR(need to next verify)

If one lidar is the first time to use, the user need to assign an IP address to this sensor.

```bash
sudo dnsmasq -C /dev/null -kd -F 192.168.100.50,192.168.100.200 -i eth0 --bind-dynamic
```

Note:  `eth0`  must be replaced by the name of your network card. It can be seen by command `ifconfig`.

Then, the user can try to ping this IP address. A successful ping illustrates that IP configuration is done.

For example:

![](/run/user/1000/doc/aeabf5f/20200902210443470.png)

## ROS Driver Running

[Here](https://static.ouster.dev/sdk-docs/ros/index.html) is an Ouster ROS driver documentation. However, it may be too complex for a beginner. So, the following will be simpler for source code download and test.

 Firstly, the user need to download source code from [ouster ROS driver](https://github.com/ouster-lidar/ouster_example ) and compile it.

```bash
mkdir ouster_ws/src
cd ouster_ws/src
catkin_init_workspace

git clone https://github.com/ouster-lidar/ouster_example.git

cd ..
catkin_make -DCMAKE_BUILD_TYPE=Release

source devel/setup.bash
```

Then, I suggest the user fix the `sensor_hostname` in the `sensor.launch`. 

From:

```bash
  <arg name="sensor_hostname" default="" doc="hostname or IP in dotted decimal form of the sensor"/>
```

To:

```bash
  <arg name="sensor_hostname" default="os-122233001531.local" doc="hostname or IP in dotted decimal form of the sensor"/>
```

Finally, the user can launch the ROS driver by following command:

```bash
roslaunch ouster_ros sensor.launch
```

After all thing is done, the effect should be same as the following test demo video.  

<video src="/home/gzx/视频/ouster demo.mkv"></video>



Lidar Sensor Hostname : 122233001531

Author: Guo Zixuan

Date: 2022-09-14 14:49:26

