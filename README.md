# kong-tcp-stream-plugin-sample
Sample Plugin for Kong TCP Stream

#Pre-requisites

A Running Kong instance configured to support the TCP stream mode: https://konghq.com/blog/tcp-stream-support-kong/

#Usage

1. Clone this repo
2. Update the following Kong environment variables to inject the plugin on startup

```code
KONG_PLUGINS=bundled,myplugin
```

where,  ```myplugin``` is the name of this sample plugin

Set the Lua Package Path to the path to the plugin

```code
KONG_LUA_PACKAGE_PATH=/opt/conf/kong-plugins/?.lua;
```

4. Start Kong

5. Enable the plugin in your Kong configuration for the appropriate service/route
6. The plugin will write a log line for each phase of the TCP connection
