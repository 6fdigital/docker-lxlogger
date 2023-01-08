# Docker Image for LxLogger

A LxLogger docker image containing InfluxDB and Grafana in the 
following versions:

| Description  | Value  |
|--------------|--------|
| InfluxDB     | 1.8.10 |
| Grafana      | 9.3.2  |

## Quick Start

To use this image, you need to have Docker installed on your system. You can 
find the installation instructions [here](https://docs.docker.com/get-docker/).
Also, you need a valid lxlogger download from our shop 
[here](https://www.lxlogger.de/) which contains the `lxlogger` binary. This will
work with all available editions of LxLogger.

Create the following folders on your docker host system. We need a `influxdb`, 
`grafana` and `lxlogger` folder:

* ~/docker/lxlogger/lxlogger
* ~/docker/lxlogger/influxdb
* ~/docker/lxlogger/grafana

Download a copy of the lxlogger- configuration template 
([.lxlogger.toml](https://github.com/6fdigital/docker-lxlogger/blob/master/lxlogger/.lxlogger.toml))
file and save it to the `~/docker/lxlogger/lxlogger` folder. Open the file and 
provide the Hostname of your Loxone® Miniserver and a valid username and 
password.

Then unzip the lxlogger download (`lxlogger_<edtion>_linux_<arch>.zip`) and copy 
the `lxlogger` binary to your `~/docker/lxlogger/lxlogger` folder. Keep in mind 
to use the correct binary for your system architecture.

Now you're ready to start the docker container. Open a terminal and execute the
following command (be sure to replace the folder path's with your own):

```sh
docker run -d \
  --name lxlogger \
  -p 3003:3003 \
  -p 8086:8086 \
  -v <path-to-lxlogger-folder>/:/usr/lib/lxlogger \
  -v <path-to-influx-folder>/influxdb/:/var/lib/influxdb \
  -v <path-to-grafana-folder>/grafana/:/var/lib/grafana \
  6fdigital/docker-lxlogger:latest
```

## Commands
To control the state of the container run:
```sh
# stop container
docker stop lxlogger

# start container
docker start lxlogger

# restart container
docker restart lxlogger

# open terminal in container
docker exec -it lxlogger bash
```

## Details
### LxLogger

You're able to open a ssh connection to the docker container to get access to
the lxlogger binary. This gives you the ability to run the different available 
lxlogger commands.

```sh
# get the name of your container
docker ps
# open a new terminal within the lxlogger folder
docker exec -w /usr/lib/lxlogger -it <container-name> /bin/bash

# Now you're inside the lxlogger folder
# of you're lxlogger container:
#
# check your lxlogger configuration file options
./lxlogger config check
# import statistic data
./lxlogger import -y 2022 -m 12
# get info
./lxlogger info
```

For more information about the available lxlogger commands, please visit our
support website [here](https://support.lxlogger.de/docs/index/).

### Grafana

Open <http://localhost:3003>

```
Username: root
Password: root
```

#### Add data source in Grafana

1. Using the wizard click on `Add data source`
2. Choose a `name` for the source and flag it as `Default`
3. Choose `InfluxDB` as `type`
4. Choose `direct` as `access`
5. Fill remaining fields as follows and click on `Add` without altering other fields

Basic auth and credentials must be left unflagged. Proxy is not required.

Now you are ready to add your first dashboard and launch some queries on a
database.

### InfluxDB

```
Username: root
Password: root
Port: 8086
```

#### InfluxDB Shell (CLI)

1. Establish a ssh connection with the container (see in LxLogger section)
2. Launch `influx` to open InfluxDB Shell (CLI)

## Development

## Pre-requisites

```sh
# create a new builder for getting access to the docker 
# buildx command (new multi-architecture features) and 
# to use the builder for further builds
docker buildx create --name <my-builder> --use
```
### Build
```sh
# build
docker buildx build --platform=linux/amd64,linux/arm/v8 .
# build and push
docker buildx build --platform=linux/amd64,linux/arm/v8 -t 6fdigital/docker-lxlogger:latest --push .
```
