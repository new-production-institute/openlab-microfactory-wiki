# How to interact with the Resource Monitoring System

This guide assumes that you have successfully completed the instrucction in the [installation guide](installation.md).

Now that the system is up, let's interact with the different services.


## Prerequisites

**Local ip address of the server**  

Most probably you already know the ip address of the server, but if that is not the case, 
and you still have access to a terminal window of your server, 
it is possible to get the local ip address running the following command:

`ipconfig getifaddr en0`

**InfluxDB setup**

One-time setup of the database is required: [influxdb_instructions.md](influxdb_instructions.md)

## Interacting with the services

The two main services that you will use are NodeRed and Grafana.
The other two services, InfluxDB and Mosquitto are there to store the data and transmit the data between sensors and services.

### NodeRed

[NodeRed](https://nodered.org) is an open-source low-code programming enviroment for real-time data manipulation.

NodeRed is being exposed in port 1880, therefore, in your browser go to:  
**< server-ip-address >:1880**

In the installed template there are some examples on how to get sensor data, calculate new values from them, 
and store them in the Influx database.

[nodered_instructions.md](nodered_instructions.md) provides more detailed explanations on how to use the templates and how to modify them for your own needs.

### Grafana

[Grafana](https://grafana.com/oss/grafana/) is an open-source software for creating interactive dashboards.

Grafana is being exposed in port 3000, therefore, in your browser go to:  
**< server-ip-address >:3000**

[grafana_instructions.md](grafana_instructions.md) provides more detailed explanations for how to use the templates and how to modify them for your own needs.

