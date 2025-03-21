# Resource Monitoring System

## Introduction 

This wiki contains a step-by-step guide for setting up a Resource Monitoring System (RMS) for Microfactories and small workshops.

An RMS is a software stack based on open-source solutions to collect, aggregate, monitor, and control machines and resource utilisation in a Microfactory.

The stack consists of:

* **Mosquitto MQTT Broker** as communication layer.  
* **Node-Red** as agreggation and data manipulation layer.
* **Timeseries database InfluxDB2** as storage layer.
* **Graphana** as visualization layer.
* (Optional) **Home Assistant** as control layer and/or alternative dashboard.
  
The stack is deployed and orchestrated via docker compose.

## Current status

The current version of the RMS monitors energy consumption and machine utilisation.  
Energy tracking is done with off-the-shelf smart plugs, while job data for Prusa 3D printers is tracked using the [PrusaLink API](https://help.prusa3d.com/article/prusa-connect-and-prusalink-explained_302608).

## Outlook

In the future additional resource flows might be included, such as material flows, energy sources (own photovoltaic electricity generation vs grid), etc.

