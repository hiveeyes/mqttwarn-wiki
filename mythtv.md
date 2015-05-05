This service allows for on-screen notification popups on [MythTV](http://www.mythtv.org/) instances. Each target requires
the address and port of the MythTV backend instance (&lt;hostname&gt;:&lt;port&gt;), and a broadcast address.

```ini
[config:mythtv]
timeout = 10  # duration of notification
targets = {
                          # host:port,            broadcast address
    'all'               :  [ '192.168.1.40:6544', '192.168.1.255'],
    'frontend_bedroom'  :  [ '192.168.1.40:6544', '192.168.1.74' ] }
    }
```

| Topic option  |  M/O   | Description                           |
| ------------- | :----: | --------------------------------------|
| `title`       |   O    | notification title (dflt: `mqttwarn`) |
| `image`       |   O    | notification image url                |