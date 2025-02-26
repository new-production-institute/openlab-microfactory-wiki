# Resource Monitoring System

## Introduciton 

This wiki contains a step-by-step guide for setting up a Resource Monitoring System (RMS) for Microfactories.

An RMS is a software stack based on open-soruce solutions to collect, aggregate, monitor, and control machines and resoruce utilisation in a Microfactory.

The stack consists of:

* Communication Layer: Mosquitto MQTT Broker
* Agreggation and Data Manipulation Layer: NodeRed
* Storage Layer: Timeseries database InfluxDB2
* Display Layer: Graphana Dashboard
* (Optional) Control Layer: Home Assisatant
  
The stack is deployed and orchestrated via docker compose.

## Current status

In the current version of the stack, the RMS monitors energy consumption per machine with the help of off-the-shelf smart plugs, and job data for Prusa 3D printers.

## The future

In the future additional resource flows might be inlcuded, such as material flows, machine up/down time, etc.

