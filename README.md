# apcupsd-influxdb-v2-exporter

Dockerized Python script that will send data from [apcupsd](http://www.apcupsd.org/) to [influxdb](https://hub.docker.com/_/influxdb).
This is a fork from [atribe/apcupsd-influxdb-exporter](https://github.com/atribe/apcupsd-influxdb-exporter) modified to work with InfluxDB v2.

## How to build
Building the image is straight forward:
* Git clone this repo
* `docker build -t apcupsd-influxdb-exporter  .`

## Environment Variables
These are all the available environment variables, along with some example values, and a description.

| Environment Varialbe | Example Value                 | Description                                                                                                             |
| -------------------- |-------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| WATTS | 1000                          | if your ups doesn't have NOMPOWER, set this to be the rated max power, if you do have NOMPOWER, don't set this variable |
| APCUPSD_HOST | 192.168.1.100                 | host running apcupsd, defaults to the value of influxdb_host                                                            |
| HOSTNAME | unraid                        | host you want to show up in influxdb. Optional, defaults to apcupsd hostname value                                      |
| INFLUXDB_ORG | MyOrg                         | InfluxDB v2 organization name                                                                                           |
| INFLUXDB_TOKEN | shg!h3v6vbe                   | InfluxDB v2 access token                                                                                                |
| INFLUXDB_URL | http://192.168.1.100:812/influxdb | InfluxDB v2 host url                                                                                                    |
| INFLUXDB_BUCKET | 8086                          | InfluxDB v2 bucket name                                                                                              |
| INTERVAL | 10                            | optional, defaults to 10 seconds                                                                                        |
| VERBOSE | true                          | if anything but true docker logging will show no output                                                                 |

## How to Use

### Run docker container directly
```bash
docker run --rm  -d --name="apcupsd-influxdb-v2-exporter" \
    -e "WATTS=600" \
    -e "INFLUXDB_HOST=10.0.1.11" \
    -e "APCUPSD_HOST=10.0.1.11" \
    -t daviddual/apcupsd-influxdb-exporter
```
Note: if your UPS does not include the NOMPOWER metric, you will need to include the WATTS environment variable in order to compute the live-power consumption 
metric.

### Run from docker-compose
```bash
version: '3'
services:
  apcupsd-influxdb-exporter:
    image: daviddual/apcupsd-influxdb-exporter
    container_name: apcupsd-influxdb-exporter
    restart: always
    environment:
      WATTS: 1000
      APCUPSD_HOST: 10.0.1.11
      INFLUXDB_HOST: 10.0.1.11
      INTERVAL: 5
```

If you want to debug the apcaccess output or the send to influxdb, set the environment variable "VERBOSE" to "true"
