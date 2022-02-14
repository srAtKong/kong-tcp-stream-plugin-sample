# kong-tcp-stream-plugin-sample
Sample Plugin for Kong TCP Stream

# Pre-requisites

A Running Kong instance configured to support the TCP stream mode: https://konghq.com/blog/tcp-stream-support-kong/

# Usage

1. Clone this repo
2. Update the following Kong environment variables to inject the plugin on startup

```code
KONG_PLUGINS=bundled,myplugin
```

where,  ```myplugin``` is the name of this sample plugin

If running Kong on Docker, craate a docker volume to mount the plugin files - eg I have mounted the plugin files to ```/opt/kong/kong-plugins```

Set the Lua Package Path to the path to the plugin

```code
KONG_LUA_PACKAGE_PATH=/opt/conf/kong-plugins/?.lua;
```

4. Start Kong

5. Enable the plugin in your Kong configuration for the appropriate service/route
6. The plugin will write a log line for each phase of the TCP connection

```code
kong    | 2022/02/14 14:48:50 [error] 100#0: *4 [kong] handler.lua:31 [myplugin] saying hi from the 'init_worker' phase, context: init_worker_by_lua*
kong    | 2022/02/14 14:48:55 [info] 100#0: *57 client 172.30.0.1:43926 connected to 0.0.0.0:5555
kong    | 2022/02/14 14:48:55 [error] 100#0: *57 [kong] handler.lua:40 [myplugin] saying hi from the 'preread' phase while prereading client data, client: 172.30.0.1, server: 0.0.0.0:5555
kong    | 2022/02/14 14:48:55 [info] 100#0: *57 proxy 172.30.0.2:56304 connected to 45.79.112.203:4242
kong    | 2022/02/14 14:49:06 [info] 100#0: *57 client disconnected, bytes from/to client:8/8, bytes from/to upstream:8/8
kong    | 2022/02/14 14:49:06 [error] 100#0: *57 [kong] handler.lua:58 [myplugin] saying hi from the 'log' phase while proxying connection, client: 172.30.0.1, server: 0.0.0.0:5555, upstream: "45.79.112.203:4242", bytes from/to client:8/8, bytes from/to upstream:8/0
```
