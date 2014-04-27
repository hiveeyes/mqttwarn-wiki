This service allows for on-screen notification popups on [XBMC](http://xbmc.org/) instances. Each target requires the address and port of the XBMC instance (<hostname>:<port>), and an optional username and password if authentication is required.

```ini
[config:xbmc]
targets = {
                          # host:port,           [user], [password]
    'living_with_auth' :  [ '192.168.1.40:8080', 'xbmc', 'xbmc' ],
    'bedroom_no_auth'  :  [ '192.168.1.41:8080' ] }
    }
```

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `title`       |   O    | notification title                     |
| `image`       |   O    | notification image url  ([example](https://github.com/jpmens/mqttwarn/issues/53#issuecomment-39691429))|

Requires: 
* A running [XBMC](http://xbmc.org/) instance