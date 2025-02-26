# InlfuxDB2 setup instrucitons

To start using the system it is necessary to do a one-time setup of the database.
In this instructions you will learn how to create data storage spaces called "buckets".

# Login to InfluxDB
InfluxDB is exposed by default in port `8087`.  
Assuming that you know the [local ip address](./general_instructions.md#prerequisites) of your server,
got to the following address in a web browser:  

> < server-ip-address >:8087  

The default credentials are:


> user: me  
> pwd: mypassword

These credentials can be changed by adjusting the environment variables in the `docker_compose.yml` file under the section for influxdb2.  

# Setting up a bucket

