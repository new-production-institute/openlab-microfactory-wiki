# How to install the Resource Monitoring System

## Base system

**Option 1:  Raspberry Pi**

Minimal hardware verison: Raspberry Pi 4  

Operating System: Raspberry Pi OS Lite (64-bit)  

The easiest way to setup a new SD card with Raspberry Pi OS is with the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/).

**Option 2: Mini PC as Server**  

For this option I would recommend to install [Proxmox](https://www.proxmox.com/en/downloads "Proxmox.com") as Operating System.  

Proxmox is a open-source server management platform. With it, its possible to deploy and manage multiple Virtual Machines (VM) and Linux Containers (LXC) in a easy and uncumplicated way.

[Here](https://phoenixnap.com/kb/install-proxmox) you can find a step-by-step guide if needed.

To deploy an Debian VM I would recommend to use the [**VE Helper-Scripts**](https://tteck.github.io/Proxmox/#debian-12-vm).

However, if you already have your own server running this is not necessary and you can deploy the resoruce monitoring system in your current setup.



## Prerequisits

### Docker and Docker Compose

**Option 1: Manual installation**

[Official documentation](https://docs.docker.com/engine/install/ubuntu/)

**Option 2: IoTStack installation (recommended)**

[IoTStack](https://sensorsiot.github.io/IOTstack/) is a builder for docker-compose files. It also takes care that Docker and Docker Compose are installed in the system and configured correctly.

In the past I have used the [add-on method](https://sensorsiot.github.io/IOTstack/Basic_setup/#addonInstall) successfully, but feel free to use the PiBuilder method if you prefer.


## Using the Resource Monitoring Template

1.  Create a directory for the Resoruce Monitoring System, e.g.:

``` 
mkdir resource_monitoring 
cd resource_monitoring

```

2. Copy the content of the **\template** folder of this repository in the folder that you just created.


3. Start Docker Compose

```
docker-compose up -d
```


**Alternative**

As an alternative, if you installed IoTStack you could use it to create your own docker-compose.yml file.

1. Create the docker-compose.yml file with the IOTStack builder

``` 
cd IOTStack
./menu.sh
```

Follow the menu to create your stack.
For the resoruce monitoring system it is necessary to select:

* NodeRed
* Mosquitto
* InlfuxDB2 (IMPORTAT: Make sure to select **InfluxDB2** and NOT InfluxDB)
* Grafana
* (Optional) Home Assistant

Feel free to add other applicaitons/services that you might need.

The output of the IOTStack builder will be a docker-compose.yml file with the services you selected.


2. Copy the templates

We provide ready-to-use templates for collecting data from Shelly smart energy plugs, newer Prusa printters (PrusaMini, MK4, Core One, Prusa XL)
as well as data aggregation flows and a sample Graphana dashboard.

Copy the templates from HERE to your **\IOTStack** directory.


3. Start Docker Compose

```
docker-compose up -d
```
