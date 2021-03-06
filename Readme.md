# Robots

## Overview:

ROS Package that simulates a battery manager system for a fleet of robots. 

## Architecture
---
![Alt text](doc/diagram.png?raw=true "Manager Robot interfaces")

The goal for this project is to create a manager system that deals with several robots. To accomplish this, two type of nodes were created: 
- **Manager Node**
 
- **Robot Node**   

These two type of nodes communicate via two services:
- **Register Service**:
  - Service provided by the **Manager node** used to register a specific robot in the Manager list. 

- **Control Service**:
  - Service provided by the **Robot node** that enables the user (Manager) to control the robot. 

To simulate the battery a class Battery was created. Based on the charge and discharge rates it calculates the % of battery 
based on the elapsed time.

![Alt text](doc/RobotBattery.png?raw=true "Robot Battery")

## How to run:

### Prerequisite:

The package was developed with the following setup:

>**Distributor ID:**	*Ubuntu*

>**Release:**	*18.04*

>**Codename:**	*bionic*

>**ROS Distribution:** *Melodic*

### Install:

```sh
$ mkdir -p ~/robot_ws/src && cd ~/robot_ws/src/
$ git clone https://github.com/carlosmendeskk/robots.git robots
$ cd ~/robot_ws/
$ catkin_make
```

### Run:

There are multiple ways to launch the simulation. 

**Launch Files and params**:

 - *Robot Launch Params*:
  
| Name                  	| Min Value 	| Max Value 	| default value 	|
|-----------------------	|-----------	|-----------	|---------------	|
| discharge_rate  [%/min]  	| 1.0       	| 58.82     	| 58.82         	|
| initial_battery_level [%] | 0.0       	| 100.0     	| 100           	|
| charge_rate [%/min]      	| 1.0       	| 100.0     	| 100           	|
---
 - *Manager Launch Params*:
  
| Name                  	| Min Value 	| Max Value 	| default value 	|
|-----------------------	|-----------	|-----------	|---------------	|
| print_interval_seconds   	| 1.0       	| 60        	| 1.0           	|

--- 
-  simulation.launch - Spawns the manager node and two robots.
-  manager.launch - Spawns only the manager node.
-  robot.launch - Spawns only a robot node. It can be spawned multiple times.

**Running the simulation.launch**:
```
$ source ~/robot_ws/devel/setup.bash
$ roslaunch robots simulation.launch
```

**Notes**:
> :warning: When launching the robot, ensure that the manager node is running. If the manager Register service is not running, the robot node will only wait 5 seconds until it stops.

> :warning: If manager node dies, the robot nodes will all stop operation.

> :warning: **Maximum number of robots connected to Manager**: Currently is 10!


### Example Output:

Output for simulation.launch:

```
[INFO] [1598210720.033439]: Registered: /robot_AV2_19986_562270367572121903
[INFO] [1598210720.036225]: Registered: /robot_AV2_19986_5431562172365497896
[INFO] [1598210721.020082]: /robot_AV2_19986_5431562172365497896: charging current 100.0 %
[INFO] [1598210721.024783]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210722.013246]: /robot_AV2_19986_5431562172365497896: operating current 99.8354644775 %
[INFO] [1598210722.014752]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210723.013683]: /robot_AV2_19986_5431562172365497896: operating current 99.668762207 %
[INFO] [1598210723.016654]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210724.013351]: /robot_AV2_19986_5431562172365497896: operating current 99.5021209717 %
[INFO] [1598210724.015133]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210725.013891]: /robot_AV2_19986_5431562172365497896: operating current 99.3354187012 %
[INFO] [1598210725.017635]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210726.013927]: /robot_AV2_19986_5431562172365497896: operating current 99.1687393188 %
[INFO] [1598210726.017701]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210727.014017]: /robot_AV2_19986_5431562172365497896: operating current 99.0020599365 %
[INFO] [1598210727.017720]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210728.014014]: /robot_AV2_19986_5431562172365497896: operating current 98.835395813 %
[INFO] [1598210728.017688]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
[INFO] [1598210729.013967]: /robot_AV2_19986_5431562172365497896: operating current 98.6687393188 %
[INFO] [1598210729.017633]: /robot_AV2_19986_562270367572121903: charging current 100.0 %
```

## Future Work:
-   Add retry mechanism into robot to reconnect with manager if it gets disconnected.
-   Extend the Control service to support several type of method ids.
-   Add state machine to robot class.
