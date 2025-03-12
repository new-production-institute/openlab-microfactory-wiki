# Configuration of the Resource Monitoring System

The two main services from the user's perspective are NodeRed and Grafana.  
The other two services, InfluxDB and Mosquitto, are there to store and transmit the data between sensors and services.

This guide assumes that you have successfully completed the instrucction in the [installation guide](installation.md).  
With the system up we still have to configure some aspects to make it work.

## InfluxDB configuration

**Setup an InfluxDB2 Bucket**

1. Access the InfluxDB UI
   Open your browser and navigate to your InfluxDB2 instance: http://rms:8086
2. Log in to InfluxDB
   Enter the creadentials. By default in the docker-compose.yml example file, the InfluxDB credentials are:

   ```
   user: me
   password: mypassword
   ```
   You can always change them in the docker-compose.yml file.
   
3. Create a the new Buckets we need
   
   Repeat the steps below two times, one for bucket name `rms_bucket` and one more time for bucket name `rms_bucket_long`.

   For bucket `rms_bucket` use retention period of 7 days.  
   For bucket `rms_bucket_long` use retention period of forever.
   
   * Go to "Load Data" > "Buckets" in the left panel.
   * Click "Create Bucket".
   * Provide a Bucket name.
   * Set the Retention Period.
   * Click "Create".


**Create Access Token**  

Now we need to create an access token for the buckets.

* Go to "Load Data" > "API Tokens" in the left panel.
* Click "Generate API Token" > "Custom API Token".
* Give a meaningful name, e.g. "Node-Red Token"
* Select "Read" and "Write" for the newly created buckets (`rms_bucket`, `rms_bucket_long`).
* Click "Generate".
  
Copy and save the token securely—you’ll need it for Node-Red.

## Node-red configuration

**Connect Node-Red to InfluxDB**

* Open Node-Red http://rms:1880.
* At the top select the tab named "shellyplug-sensors-integration".
* At the right side of all the flow there is a node called "InfluxDB2".
* Double-click the node and click the pencil icon next to "Server".
  
In the "InfluxDB Server" configuration:  
* Host: http://rms:8086
* Organization: myorg
* Token: Paste the token generated earlier
* Version: Select InfluxDB 2.0
* Click Update

## Grafana configuration

Finally, the last required configuration is for connecting Grafana to the InfluxDB database to retrieve the data for the dashboard.

* Open Grafana http://rms:3000
* Default creadentials are `admin/admin`. After first login you will me prompted to change your password.
* On the left panel click on the arrow to open the menu under "Connections".
* Click on "Data sources"
* On the top-right click on "+ Add new data source".

In the new panel fill the following fields:

* Name: rms influxdb
* Query language: Flux
* URL: http://rms:8087
* Organization: myorg
* Token: Paste the token generated earlier for InlfuxDB
* Default Bucket: rms_bucket

The rest of the fields stay as they are.

---

## Understanding the services


### Node-Red

[Node-Red](https://nodered.org) is an open-source low-code programming enviroment for real-time data manipulation.

In the installed template, there are some examples on how to get sensor data, calculate new values from them, 
and store them in the Influx database.

[Node-Red template](nodered_instructions.md) provides more detailed explanations on how to use the templates and how to modify them for your own needs.

### Grafana

[Grafana](https://grafana.com/oss/grafana/) is an open-source software for creating interactive dashboards.


[Grafana template](grafana_instructions.md) provides more detailed explanations for how to use the templates and how to modify them for your own needs.

