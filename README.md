# kong-tcp-stream-plugin-sample
## Sample Plugin for Kong TCP Stream

The plugin will write a log line for each phase of the TCP connection.

The lua files for the plugin will be mounted in the Kong EE container running in dbless mode, and the files will be included in `KONG_LUA_PACKAGE_PATH`. All of this is done in the Docker Compose file.

## Kong Declarative Config

The declarative configuration file for this example is present in the `dbless_config` directory. It defines one service (tcpbin) and one TCP route, with the plugin applied to it

# Pre-requisites

This sample uses Kong EE. A valid license for Kong-EE should be set to the environment variable $KONG_LICENSE_DATA
Or if you want to use Kong CE, update the container image in the docker compose file

Docker, Docker-Compose

# Usage

## Setting up

1. Clone this repo and navigate to it
2. Start Kong with  `docker-compose up -d`

```shell
docker-compose up -d
```

## Verify whether containers are up or not

```shell
$ docker ps

CONTAINER ID   IMAGE                              COMMAND                  CREATED              STATUS                        PORTS                                                                                                                                         NAMES
fde11510014d   kong/kong-gateway:latest   "/docker-entrypoint.…"   7 minutes ago   Up 7 minutes (healthy)   0.0.0.0:5555->5555/tcp, 0.0.0.0:8000-8004->8000-8004/tcp, 0.0.0.0:8443-8445->8443-8445/tcp, 8446-8447/tcp   kong

## Test

```shell
nc localhost 5555
a
a
b
b
```
The plugin will write a log line for each plugin phase:

```code
kong  | 2022/02/24 12:19:09 [error] 89#0: *4 [kong] handler.lua:31 [myplugin] saying hi from the 'init_worker' phase, context: init_worker_by_lua*
kong  | 2022/02/24 12:19:14 [info] 90#0: *57 client 172.24.0.1:53266 connected to 0.0.0.0:5555
kong  | 2022/02/24 12:19:14 [error] 90#0: *57 [kong] handler.lua:40 [myplugin] saying hi from the 'preread' phase while prereading client data, client: 172.24.0.1, server: 0.0.0.0:5555
kong  | 2022/02/24 12:19:14 [info] 90#0: *57 proxy 172.24.0.2:51284 connected to 45.79.112.203:4242
kong  | 2022/02/24 12:19:19 [info] 90#0: *57 client disconnected, bytes from/to client:4/4, bytes from/to upstream:4/4
kong  | 2022/02/24 12:19:19 [error] 90#0: *57 [kong] handler.lua:58 [myplugin] saying hi from the 'log' phase while proxying connection, client: 172.24.0.1, server: 0.0.0.0:5555, upstream: "45.79.112.203:4242", bytes from/to client:4/4, bytes from/to upstream:4/0
```
