# Device Controller

The deviceController module gives users of microWaggle nodes a form of remote user control. The module is capable of changing frequencies of reading and publishing data as well as enabling & disabling of device sensors. 
 
## Micro-Waggle deviceController CLI guide

The deviceController module lets you manage a microWaggle node network through a CLI. It should be noted that the **access tokens** and the unique **device IDs** privided by Particle.io needs to be at hand before the implimentation of the module. 

## First time usage: 
- Run `controller.py` with no args. -> `python3 controller.py`. The`--help` prompt gives you the basic options that the controller provides.
- On its initial run`controller.py` will create `nodeConfig.json` file which keeps the node configuration the user intends to have. Everytime the module is implimented, it will seek to mimic the configuration defined on `nodeConfig.json`on the actual microWaggle network.  

### Local Node Configuration
The command  `python3 controller.py --add` is intened to be used in adding new nodes to the local configuration file. An example usage of the said command is given below:
```bash
python3 controller.py --add

-----------------New Node Parametors------------------------
Node Name? microWaggle_1
Node ID? 0001
Device ID? 53002a000c51353432383931
Data Publish Frequency? 10
Status Publish Frequency? 15
SD Card Active? [True/False] True
Access Token ? c9003c4f929c03b67daac131a84b9d3aa3d75e3e
Use the default Micro-waggle Config? [True/False] True
Updating Nodes
---------------Micro-Waggle-Controller----------------
[{'config': [['tempSensor', 1, True, 10], ['humiditySensor', 2, True, 10]],
  'deviceID': '53002a000c51353432383931',
  'enabled': True,
  'name': 'microWaggle_1',
  'nodeID': '0001',
  'saveToSD': True,
  'sendingFrequency': '10',
  'statusFrequency': '15',
  'token': 'c9003c4f929c03b67daac131a84b9d3aa3d75e3e'}]
------------------------------------------------------
```
As indicated above, the user will be prompted to provide the following details for a given node:
- Node Name : User defined name for the intened microWaggle node **(Cannot have spaces)**
- Node ID   : User defined name for the intened microWaggle node **(Cannot have spaces)**
- Device ID : The Device ID provided by Particle.io for the intened node
- Data Publish Frequency : The frequency in which sensor data should be published to the Particle.io cloud
- Status Publish Frequency: The frequency in which node status data should be published to the Particle.io cloud
- Access Token : The access token provided by Particle.io for the intened node
- Use the default Micro-waggle Config: The desired configuration of the sensors present on the node. **First time users are recommended to keep the default microWaggle Configuration**. If you choose to keep a diffent configuration the following information needs to be provided.
- Number of Sensors to be added : The number of sensors present in the actual microWaggle code 
- Names of each sensor : Use specified names for each sensor **(Cannot have spaces)**
- ID for sensor : The sensor ID's specified on the firmware of the microWaggle device for the intended sensor **(Cannot have spaces)**
- Enabled for sensor : State wheather the sensor is enabled or disabled on the specific sensor
- Sensing Frequency for sensor : State how often the sensor needs to read data
An example usage of the prompt is given below:

```bash
Use the default Micro-waggle Config? [True/False] False
Number of Sensors to Add ? 2
------------------------------------
Configuring Sensor 1:
Name for sensor 1? lightSensor
ID for sensor 1? 3  
Enabled for sensor 1?(True/False) True
Sensing Frequency for sensor 1? 18
------------------------------------
Configuration for Sensor 1:
['lightSensor', '3', True, '18']
------------------------------------
------------------------------------
Configuring Sensor 2:
Name for sensor 2? pressureSensor
ID for sensor 2? 3
Enabled for sensor 2?(True/False) False
Sensing Frequency for sensor 2? 25
------------------------------------
Configuration for Sensor 2:
['pressureSensor', '3', False, '25']
------------------------------------
Final Configuration for the Sensors
[['lightSensor', '3', True, '18'], ['pressureSensor', '3', False, '25']]
------------------------------------
```
Once a local configuration is set `python3 controller.py --list` command can be used to verify the intened configuration. An example configuration is given below:

```bash
python3 controller.py --list
[✓] Node with ID 0001 loaded
[✓] Node with ID 0002 loaded
---------------Micro-Waggle-Controller----------------
[{'config': [['tempSensor', 1, True, 10], ['humiditySensor', 2, True, 10]],
  'deviceID': '53002a000c51353432383931',
  'enabled': True,
  'name': 'microWaggle_1',
  'nodeID': '0001',
  'saveToSD': True,
  'sendingFrequency': '10',
  'statusFrequency': '15',
  'token': 'c9003c4f929c03b67daac131a84b9d3aa3d75e3e'},
 {'config': [['lightSensor', '3', True, '18'],
             ['pressureSensor', '3', False, '25']],
  'deviceID': '53002a000c51353432383932',
  'enabled': True,
  'name': 'microWaggle_2',
  'nodeID': '0002',
  'saveToSD': False,
  'sendingFrequency': '15',
  'statusFrequency': '20',
  'token': 'c9003c4f929c03b67daac131a84b9d3aa3d75abb'}]
------------------------------------------------------
```

## Basic use cases

- `--help` : Lists all commands, as well as their respective syntaxes and functions.
   - Ex. Usage: `python3 controller.py --help` 

- `--list`: Lists all nodes present on the local config file. The respective information such as nodeID and deviceID, and sensor configuration will also be displayed.
  - Ex. Usage: `python3 controller.py --list`

- `--enAll` : Enables all sensors on on all nodes.
   - Ex. Usage: `python3 controller.py --enAll`

- `--disAll` : Disables all sensors on all nodes.
   - Ex. Usage: `python3 controller.py --disAll`

- `--rm <nodeID>` : removes the node with the specified name
   - Ex. Usage: **Given a node with nodeID 33 exists** : `python3 controller.py --rm <nodeID>`

- `--add` : Starts the prompt to add a new node.
   - Ex. Usage: `python3 controller.py --add`

- `--configure <nodeID> [-rn/-id/-d/-e/-sdf/-ssf/-sm/-sd/-sen]`
   - Ex. Usage: **See below**

## The 'configure' command

The configure command lets you remotely control your nodes and there sensors after the intended nodes is included on the local config file. 

- `-rn <new name>` : Renames the entered node.
  **NODE NAMES CANNOT HAVE SPACES!**
  - Ex. Usage: **Given a node with the nodeID 33 exists**: `python3 controller.py --configure 33 -rn bar` -> Node 33 is now named bar.
  - Ex. Usage: **Given nodes with the nodeID 33, 99, and 66 exist**: `python3 controller.py --configure 33,66,99 -rn Field_node` -> nodes with the nodeID 33, 66, and 99 are now named "Field_node".

- `-id <new nodeID>` : changes the nodeID of the entered node. 
  - Ex. Usage: **Given a node with the nodeID 33 exists**: `python3 controller.py --configure 33 -id 96024` -> Node with ID 33 now has the nodeID 96024.
 **ANY CHANGES DONE TO A NODE WITH A SPECIFIED ID WILL BE DONE TO ALL NODES WITH THE MATCHING ID.**

- `-d` : Disables all sensors on the given node. Can disable all sensors on more than one node at once if the nodeID's are separated by a comma. Example usage:
  - Ex. Usage: **Given a node with the nodeID 34 exists**: `python3 controller.py --configure 34 -d` -> Node with ID 34 is disabled.
  - Ex. Usage: **Given nodes with the nodeID 35, 36, 37, & 38 exist**: `python3 controller.py --configure 35,36,37,38 -d` -> Sensors on nodes with ID 35, 36, 37, & 38 are disabled.

- `-e` : Enables all sensors on the given node. Can enable all sensors on more than one node at once if the nodeID's are separated by a comma. Example usage:
  - Ex. Usage: **Given a node with the nodeID 34 exists**: `python3 controller.py --configure 34 -e` -> Node with ID 34 is enabled.
  - Ex. Usage: **Given nodes with the nodeID 39, 40, 41, & 42 exist**: `python3 controller.py --configure 39,40,41,42 -e` -> Sensors on nodes with ID 39, 40, 41, & 42 are enabled.

- `-sdf` : Changes the frequency at which the selected node sends data. Can change multiple node's setting if nodeID's are separated by a comma. Example usage:
  - Ex. Usage: **Given a node with the nodeID 43 exists**: `python3 controller.py --configure 43 -sdf 15` -> Node with ID 43 now has a sending frequency of 15 seconds. 
  - Ex. Usage: **Given nodes with the nodeID 44, 45, and 46 exist**: `python3 controller.py --configure 44,45,46 -sdf 15` -> Nodes ID 44, 45, & 46 now have a sending frequency of 15 seconds. 

- `-nsf` : Changes the frequency at which the selected node sends status messages. Can change multiple node's setting if nodeID's are separated by a comma. Example usage:
  - Ex. Usage: **Given a node with the nodeID 43 exists**: `python3 controller.py --configure 43 -sdf 15` -> Node with ID 43 now has a status publish frequency of 15 seconds. 
  - Ex. Usage: **Given nodes with the nodeID 44, 45, and 46 exist**: `python3 controller.py --configure 44,45,46 -sdf 15` -> Nodes ID 44, 45, & 46 now have a status publish frequency of 15 seconds. 

- `-ssf `: Changes the frequency at which the selected node senses data(for all sensors). Can change multiple node setting if nodeID's are separated by a comma. Example usage below.
  - Ex. Usage: **Given a node with the nodeID 47 exists**: `python3 controller.py --configure 47 -ssf 15` -> Node with ID 47 now has a sensing frequency of 15 seconds. 
  - Ex. Usage: **Given nodes with the nodeID 48, 49, and 50 exist**: `python3 controller.py --configure 44,45,46 -ssf 15` -> Nodes ID 48, 49, & 50 now have a sensing frequency of 15 seconds.

- `-sd <True/False>` : Sets if the node should save data to SD storage when disconnected from Wi-Fi. True = Yes, False = No. You can use this command on multiple nodes, provided there's a comma in between the nodeIDs. Example usage below. 
  - Ex. Usage: `python3 controller.py --configure 57 -sd True` -> Node with the ID 51 will store to SD.
  - Ex. Usage: `python3 controller.py --configure 58,59,60,61,62 -sm False` -> Node with the IDs 58, 59, 60, 61, & 62 will not save to SD storage.

### The 'sen configure' commmand

The `-sen` subcommand of the `--configure` command lets the users configure individual sensors on microWaggle nodes.

- `-sen add` : Adds a new sensor to the the given node via a user command prompt. If you specify multiple nodes in this command, the sensor form you fill out will be applied to **all** nodes specified. 
   - Ex. Usage: `python3 controller.py --configure 63 -sen add
   - Ex. Usage: `python3 controller.py --configure 64,65,66,67 -sen add

- `-sen rm <sensor ID to remove>` : Removes a sensor from the given node. If you specifiy multiple nodes in this command, the sensor will be removed on each node, **provided the sensor with the corresponding ID exists**.
   - Ex. Usage: `python3 controller.py --configure 68 -sen rm 01` -> Sensor 01 gets removed from node with ID 68.
   - Ex. Usage: `python3 controller.py --configure 69, 70 -sen rm 01` -> Sensor 01 gets removed from nodes with ID's 69 & 70.

- `-sen dis <sensor ID to disable>` : Disables the selected sensor. If you specify multiple sensors using this command, all sensors specified will be affected. 
   - Ex. Usage: **Given node 71 with sensor with ID 0 exists**: `python3 controller.py --configure 71 -sen dis 0`

- `-sen en <sensor ID to disable>` : Enables the selected sensor. If you specify multiple sensors using this command, all sensors specified will be affected. 
   - Ex. Usage: **Given node 72 with sensor with ID 0 exists**: `python3 controller.py --configure 72 -sen en 0`

- `-sen freq [sensing frequency in seconds] <sensor ID to affect>` : Changes the frequency of sensing for the selected sensor.
   - Ex. Usage: **Given node 73 with sensor with ID 0 exists**: `python3 controller.py --configure 73 -sen freq 45 0`


## Micro-Waggle deviceController through the Particle.io Console

Micro-Waggle also allows users to control there devices through particles console page specif to the respective Particle.io device. The specified link should look like this: https://console.particle.io/devices/YOUR_DEVICE_ID). For example, a micro-Waggle device with a device ID '53002a000c51353432383931' will have a console page like this 'https://console.particle.io/devices/53002a000c51353432383931'. 

Once at the console page specific to your device, you will find two functions named 'nodeConfig' and 'sensorConfig' on the lower left hand side of the page. The two functions should be displayed like this:
 <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/functions.JPG">

These functions allows similar means of control as the nodeController module. The function descriptions are given below.

### nodeConfig Function 
The function allows users to manage their preferences for the node as a whole. 

- Function facilitates: 
   - Enabling/disabling all sensors on the node
   - Enabling/disabling the usage of an SD 
   - Changing the frequency of reporting sensor data
   - Changing the frequency of reporting status data
  
  
#### Parameters given:

- “enableAll”         – Enables all the sensors on the node
   - Example usage: 
    
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/enableAll.jpeg">
     
- “disableAll”        – Disables all sensors of node
   - Example usage: 
    
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/disableAll.jpeg">

- “enableSD”          – Enables the SD card
   - Example usage: 
    
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/enableSD.jpeg">
    
- “disableSD”         – Disables the SD card
   - Example usage: 
    
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/disableSD.jpeg">
    
- “freqReport-[secs]” – Changes frequency of reporting sensor data to [secs] seconds
   - Example usage: Changing the data reprting frequency to 20 seconds 
    
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/freqReport.jpeg">
 
- “statusFreq-[secs]” – Changes frequency of reporting status data to [secs] seconds
   - Example usage: Changing the status reprting frequency to 15 seconds
       
   <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/statusFreq.jpeg">

### sensorConfig Function 
The function allows users to manage their preferences for individual sensors.
 
- Function facilitates: 
   - Enabling/disabling of individual sensors
   - Changing the frequency of sensing data for individual sensors.
  
#### Parameters given:

- Format: “sensorID;status;frequency”
    - sensorID: The Sensor ID defined within the firmware flashed on the micro-Waggle device. 
    - status:
       - “en”   - Enables the specified sensor
       - “dis”  - Disables the specified sensor
       -  “_”   - No changes to the status of the sensor are made
    - frequency:
       - [secs] - Changes frequency of reporting sensor data for the specified sensor to [secs] seconds
       - “_”    - No changes to the frequency of the sensing are made

- Examples
  - Enabling sensor 2:
      
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/example1.jpeg">
    
  - Changing Sensiing frequency of Sensor 2 to 30 seconds:
  
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/example2.jpeg">
    
  - Enabling Sensor 1, while changing its sensing frequency to 10 seconds
  
    <img src="https://raw.githubusercontent.com/waggle-sensor/microWaggle/master/integrated/software/deviceController/resources/images/example3.jpeg">
  
  
  
