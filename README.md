# apcupsd-influxdb-exporter

Build an x86_64 or ARM compatible Docker image that will output commonly used UPS device statistics to an influxdb database using an included version of the 
[APCUPSd](http://www.apcupsd.org/) 
tool. Dockerfiles included for both intel and ARM (RaspberryPi or comparable) chipsets.

## How to build
Building the image is straight forward:
* Git clone this repo
* `docker build -t apcupsd-influxdb-exporter  .`

## Running
```bash
docker run --rm  -d --name="apcupsd" \
    --privileged \
    -e "HOSTNAME=entertainmentcenter" \
    -e "WATTS=600" \
    -e "INFLUXDB_DATABASE=ups" \
    -e "INFLUXDB_PORT=8086" \
    -e "INFLUXDB_HOST=10.0.1.11" \
    -e "APCUPSD_HOST=10.0.1.11" \
    -t atribe/apcupsd-influxdb-exporter
```
Note: if your UPS does not include the NOMPOWER metric, you will need to include the WATTS environment variable in order to compute the live-power consumption 
metric.

If you want to debug the apcaccess output or the send to influxdb, set the environment variable "VERBOSE" to "true"
