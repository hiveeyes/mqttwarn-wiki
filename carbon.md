The `carbon` service sends a metric to a Carbon-enabled server over TCP.

```ini
[config:carbon]
targets = {
        'c1' : [ '172.16.153.110', 2003 ],
  }

[c/#]
targets = carbon:c1
```

In this configuration, all messages published to `c/#` would be forwarded to
the Carbon server at the specified IP address and TCP port number.

The topic name is translated into a Carbon _metric_ name by replacing slashes
by periods. A timestamp is appended to the message automatically.

For example, publishing this:

```
mosquitto_pub -t c/temp/arduino -m 12
```

would result in the value `12` being used as the value for the Carbon metric
`c.temp.arduino`. The published payload may contain up to three white-space-separated
parts.

1. The carbon metric name, dot-separated (e.g. `room.temperature`) If this is omitted, the MQTT topic name will be used as described above.
2. The integer value for the metric
3. An integer timestamp (UNIX epoch time) which defaults to "now".

In other words, the following payloads are valid:

```
15					just the value (metric name will be MQTT topic)
room.living 15				metric name and value
room.living 15 1405014635		metric name, value, and timestamp
```