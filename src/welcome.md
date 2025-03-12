# Resource Monitoring System

## Introduciton 

This wiki contains a step-by-step guide for setting up a Resource Monitoring System (RMS) for Microfactories and small workshops.

An RMS is a software stack based on open-soruce solutions to collect, aggregate, monitor, and control machines and resource utilisation in a Microfactory.

The stack consists of:

* **Mosquitto MQTT Broker** as communication layer.  
* **Node-Red** as agreggation and data manipulation layer.
* **Timeseries database InfluxDB2** as storage layer.
* **Graphana** as dashboard layer.
* (Optional) **Home Assistant** as control layer and/or alternative dashboard.
  
The stack is deployed and orchestrated via docker compose.

## Current status

The current version of the RMS monitors energy consumption and machine utilisation.
Energy consumtion is tracked per machine using off-the=shelf smart plugs, and for Prusa printers it also tracks energy consumption per job.

Energy tracking is done with off-the-shelf smart plugs, while job data for Prusa 3D printers is tracked using the PrusaLink API.

## The future

In the future additional resource flows might be inlcuded, such as material flows, energy sources (own photovoltaic electricity generation vs grid), etc.

