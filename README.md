# Sound Management Demo MONICA

## Overview

This repository is a working docker compose example of whole MONICA toolchain, from data simulation to visualization. The overall use case for this is sound mangagement using Sound Level Meters and sound processing. Main components are the following:
- Common Operational Picture
    - [COP UI](https://github.com/MONICA-Project/COP-UI) - The generic MONICA COP user interface
    - [COP APi](https://github.com/MONICA-Project/COP.API) - The MONICA COP API
    - [COP Updater](https://github.com/MONICA-Project/COPUpdater) - Is the link between the COP and the IoT Devices, pushing individual updates to the COP.
    - [COP DB](https://github.com/MONICA-Project/COP.DB) - The MONICA COP DB used for storing the state of the COP.
- [DSS](https://github.com/MONICA-Project/scral-framework) - Decision Support Systems that creates incidents and also creates aggregates.
- [Sound Heat Map](https://github.com/MONICA-Project/sound-heat-map) - Uses the Sound Level Meters (SLMs) for creating a sound heat map that models the sound pressure levels in a given area.
- [GOST](https://github.com/gost/server) -Provides the IoT database
- MQTT Broker ([Eclipse Mosquitto](https://mosquitto.org/))
- [RunSimulation](https://github.com/MONICA-Project/RunSimulation) - Runs the simulation of the sound level meters.

## How to get started
### Prerequisites needed
- Docker 
- Docker Compose

### Installation and execution
Clone or download and unzip this repository to a folder on your local computer. Open a command console window and change it to the newly created folder.
The first time this system is started it is best to do it stepwise to identify any problems of already occupied network ports by following this order (Used nwtwork ports are found at the end): 

#### 1, Start up a GOST server:

```bash
docker-compose up -d gost-dashboard
```

The GOST backend Postgres database takes a few seconds to start up. Make sure that the GOST API is available at http://localhost:8090/v1.0.

![IoT Database](https://github.com/MONICA-Project/DockerSoundDemo/blob/master/img/iot-db.png "GOST") 



#### 2, Start up the Simulators:

```bash
docker-compose up -d simulatorSLM
docker-compose up -d simulatorSHM
docker-compose up -d simulatorDSS
```

Check if they have up is up and running at http://localhost:8090/v1.0/Datastreams.
You should see 41 datastreams defined.

![Datastreams](https://github.com/MONICA-Project/DockerSoundDemo/blob/master/img/simulators.png "Datastream") 

#### 3, Start the COP API
Start the service to launch the COP API:

```bash
docker-compose up -d copapi
```

Check if COP API is up and running at http://localhost:8800/

![COP API](https://github.com/MONICA-Project/staff-management-demo/blob/master/img/MONICA%20Cop%20API.PNG "COP API") 


#### 4, Start the COP UI
Start the service for the COP UI:

```bash
docker-compose up -d copui
```

Check if COP UI is up and running at http://localhost:8900/

**NB!** the userid is admin@monica-cop.com and the password SOUND2020!

![COP UI](https://github.com/MONICA-Project/DockerSoundDemo/blob/master/img/cop-ui.png "COP UI") 

#### 5, Complete the start by bringing up the rest of the components
You should see movements in stound graphs when it started
```bash
docker-compose up -d
```
Which will bring up the whole environment immediately.

#### 6, Next start
If all went well the next time you can make 
```bash
docker-compose up -d
```
Which will bring up the whole environment immediately.


Observations (both localizations and - from time to time - alerts) should start coming in to GOST, visible in the dashboard at http://localhost:8090/#/observations or directly via the API at http://localhost:8090/v1.0/Observations.


### Trouble shooting
The most likely problem that is encountered is port conflicts. If a container cannot start because of conflicting ports, stop or move the conflicting ports process and restart. The next section contains the port used by the components.
If something stops working the best solution is to restart everything again.
### TCP Ports used

The following table show the list of services with opened ports (inside subnet and forward to external connections):

| Service Name | Type Port | External Port | Internal Subnet Port |
| --------------- | --------------- | --------------- | --------------- |
| copui | Service | 8900 | 8080 | 
| copapi | Service | 8800 | 80 | 
| copupdater | Service | - | - | 
| copdb | Service (PostgresSQL database)| 9996 | 5432| 
| mosquitto | Service | 1883 | 1883| 
| dashboard | GOST API Service | 8090 | 8080|
| gost-db | GOST Database | 5432 | 5432|

### Tutorials based on this demo
The following tutorials use this demo as starting point:
- [COP UI Tutorial for Sound](https://monica-project.github.io/sections/cop-api-tutorial%20for%20sound.html)

## Affiliation
![MONICA](https://github.com/MONICA-Project/template/raw/master/monica.png)  
This work is supported by the European Commission through the [MONICA H2020 PROJECT](https://www.monica-project.eu) under grant agreement No 732350.
