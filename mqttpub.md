This service publishes a message to the broker _mqttwarn_ is connected to. (To publish a message to a _different_ broker, see `mqtt`.)

Each target requires a topic name, the desired _qos_ and a _retain_ flag.

```ini
[config:mqttpub]
targets = {
                # topic            qos     retain
    'mout1'   : [ 'mout/1',         0,     False ],
    'special' : [ 'some/{device}',  0,     False ],
  }
```

If the outgoing topic name contains transformation strings (e.g. `out/some/{temp}`) values are interpolated accordingly. Should this not be possible, e.g. because a string isn't available in the _data_, the message is _not_ published.