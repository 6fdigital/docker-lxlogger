# Docker Image for LxLogger

The image contains the following components:

| Description  | Value   |
|--------------|---------|
| InfluxDB     | 1.8.3   |
| Grafana      | 7.3.7   |

## Quick Start

To start the container run:

```sh
docker run -d \
  --name lxlogger \
  -p 3003:3003 \
  -p 8086:8086 \
  -v <path-to-influx-folder>/influxdb/:/var/lib/influxdb \
  -v <path-to-grafana-folder>/grafana/:/var/lib/grafana \
  -v <path-to-lxlogger-folder>/lxlogger/:/usr/lib/lxlogger \
  6fdigital/docker-lxlogger:latest
```

To control the state of the container run:
```sh
# stop container
docker stop lxlogger

# start container
docker start lxlogger

# restart container
docker restart lxlogger
```

## Mapped Ports

```
Host		Container		Service

3003		3003			grafana
8086		8086			influxdb
```
## SSH

```sh
docker exec -it lxlogger bash
```

## Grafana

Open <http://localhost:3003>

```
Username: root
Password: root
```

### Add data source on Grafana

1. Using the wizard click on `Add data source`
2. Choose a `name` for the source and flag it as `Default`
3. Choose `InfluxDB` as `type`
4. Choose `direct` as `access`
5. Fill remaining fields as follows and click on `Add` without altering other fields

Basic auth and credentials must be left unflagged. Proxy is not required.

Now you are ready to add your first dashboard and launch some queries on a database.

## InfluxDB

```
Username: root
Password: root
Port: 8086
```

### InfluxDB Shell (CLI)

1. Establish a ssh connection with the container
2. Launch `influx` to open InfluxDB Shell (CLI)
