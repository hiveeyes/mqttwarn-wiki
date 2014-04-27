The `irccat` target fires a message off to a listening [irccat](https://github.com/RJ/irccat/tree/master) which has a connection open on one or more IRC channels.

Each target has to be configured with the address, TCP port and channel name of the particular _irccat_ it should target.

```ini
[config:irccat]
targets = {
             # address     port   channel
   'chan1': [ '127.0.0.1', 12345, '#testchan1' ],
  }
```

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `priority`    |   O    | Colour: 0=black, 1=green, 2=red        |

The priority field can be used to indicate a message colour.

![irccat](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/irccat.png)