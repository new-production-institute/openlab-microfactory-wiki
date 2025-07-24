# How to install the Resource Monitoring System

## Base system

**Option 1:  Raspberry Pi**

Minimal hardware verison: Raspberry Pi 4  

Operating System: Raspberry Pi OS Lite (64-bit)  

The easiest way to setup a new SD card with Raspberry Pi OS is with the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/).

When selecting the Operating System in the Raspberry Pi Imager, select the 64 bit Lite version (without Graphical User Interface).

**Option 2: Dedicated PC**  

Second-hand mini pc's are a great option for this applicaiton.

When using a pc, I would recommend to install [Proxmox](https://www.proxmox.com/en/downloads "Proxmox.com") as Operating System.  

Proxmox is a open-source server management platform. Proxmox makes it very easy to deploy and manage multiple Virtual Machines (VM) and Linux Containers (LXC).

[Here](https://phoenixnap.com/kb/install-proxmox) you can find a step-by-step guide to install Proxmox as your operating system.

The easiest way to deploy a Debian VM is with the help of these community scripts: [**VE Helper-Scripts**](https://tteck.github.io/Proxmox/#debian-12-vm).

However, if you already have your own server running, this is not necessary and you can deploy the resoruce monitoring system in your current setup.



## Prerequisits

### Docker and Docker Compose

**Option 1: Manual installation**

[Official documentation](https://docs.docker.com/engine/install/ubuntu/)

**Option 2: IoTStack installation**

[IoTStack](https://sensorsiot.github.io/IOTstack/) is a builder for docker-compose files. It also takes care that Docker and Docker Compose are installed in the system and configured correctly.

In the past I have used the [add-on method](https://sensorsiot.github.io/IOTstack/Basic_setup/#addonInstall) successfully, but feel free to use the PiBuilder method if you prefer.


### Assigning a hostname

For the RMS template to work it is necessary to assign the correct hostname to your server.
In Debian based systems, it is possible to manually assign the hostname by editing the file `/etc/hostname`.
Open the file in your editor of choise, for example:

```
vim /etc/hostname
```

delete the existing name and reaplace it with:

```
rms
```

save and restart your device

```
sudo reboot
```

after rebooting go egain to the console of your server and veryfy that the hostname is correct by typing

```
hostnamectl
```

the first line should be

```
Static hostname: rms
```


## Installing and running the Resource Monitoring Template

1.  Create a directory for the Resoruce Monitoring System, e.g.:

``` 
mkdir resource_monitoring 
cd resource_monitoring

```

2. Create a file inside the directory:

`touch docker-compose.yml`

3. Copy the **template** below into the file that you just created.

```
networks:
  default:
    driver: bridge

services:

  grafana:
    container_name: grafana
    image: grafana/grafana
    restart: unless-stopped
    user: "0"
    ports:
    - "3000:3000"
    environment:
    - TZ=Etc/UTC
    - GF_PATHS_DATA=/var/lib/grafana
    - GF_PATHS_LOGS=/var/log/grafana
    volumes:
    - ./volumes/grafana/data:/var/lib/grafana
    - ./volumes/grafana/log:/var/log/grafana
    healthcheck:
      test: ["CMD", "wget", "-O", "/dev/null", "http://localhost:3000"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s

  influxdb2:
    container_name: influxdb2
    image: "influxdb:latest"
    restart: unless-stopped
    environment:
    - TZ=Etc/UTC
    - DOCKER_INFLUXDB_INIT_USERNAME=me
    - DOCKER_INFLUXDB_INIT_PASSWORD=mypassword
    - DOCKER_INFLUXDB_INIT_ORG=myorg
    - DOCKER_INFLUXDB_INIT_BUCKET=mybucket
    - DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=my-super-secret-auth-token
    - DOCKER_INFLUXDB_INIT_MODE=setup
    ports:
    - "8087:8086"
    volumes:
    - ./volumes/influxdb2/data:/var/lib/influxdb2
    - ./volumes/influxdb2/config:/etc/influxdb2
    - ./volumes/influxdb2/backup:/var/lib/backup
    healthcheck:
      test: ["CMD", "influx", "ping"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s

  mosquitto:
    container_name: mosquitto
    image: eclipse-mosquitto:latest
    restart: unless-stopped
    environment:
    - TZ=${TZ:-Etc/UTC}
    ports:
    - "1883:1883"
    volumes:
    - ./volumes/mosquitto/config:/mosquitto/config
    - ./volumes/mosquitto/data:/mosquitto/data
    - ./volumes/mosquitto/log:/mosquitto/log
    - ./volumes/mosquitto/pwfile:/mosquitto/pwfile

  nodered:
    container_name: nodered
    build:
      context: ./services/nodered/.
      args:
      - DOCKERHUB_TAG=latest
      - EXTRA_PACKAGES=
    restart: unless-stopped
    user: "0"
    environment:
    - TZ=${TZ:-Etc/UTC}
    ports:
    - "1880:1880"
    volumes:
    - ./volumes/nodered/data:/data
    - ./volumes/nodered/ssh:/root/.ssh
```


4. Start Docker Compose

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

* Node-Red
* Mosquitto
* InlfuxDB2 (IMPORTAT: Make sure to select **InfluxDB2** and NOT InfluxDB)
* Grafana
* (Optional) Home Assistant

Feel free to add other applicaitons/services of your choise.

The output of the IOTStack builder will be a docker-compose.yml file with the services you selected.


2. Copy the templates

We provide ready-to-use templates for:
* collect data from Shelly smart energy plugs
* collect data from PrusaLink (PrusaMini, MK4, Core One, Prusa XL)
* data aggregation flows
* graphana dashboard

Copy the templates from HERE to your **\IOTStack** directory.


3. Start Docker Compose

```
docker-compose up -d
```
