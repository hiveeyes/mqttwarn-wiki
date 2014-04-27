The `syslog` service transfers MQTT messages to a local syslog server.

```ini
[config:syslog]
targets = {
              # facility    priority,  option
    'user'   : ['user',     'debug',  'pid'],
    'kernel' : ['kernel',   'warn',   'pid']
    }
```

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `title`       |   O    | application title (dflt: `mqttwarn`)   |

Output:
```
Apr 22 12:42:42 mqttest019 mqttwarn[9484]: Disk utilization: 94%
```