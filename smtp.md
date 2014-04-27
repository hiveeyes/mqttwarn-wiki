The `smtp` service basically implements an MQTT to SMTP gateway which needs
configuration.

```ini
[config:smtp]
server  =  'localhost:25'
sender  =  "MQTTwarn <jpm@localhost>"
username  =  None
password  =  None
starttls  =  False
targets = {
    'localj'     : [ 'jpm@localhost' ],
    'special'    : [ 'ben@gmail', 'suzie@example.net' ]
    }
```

Targets may contain more than one recipient, in which case all specified recipients get the message.

| Topic option  |  M/O   | Description                                     |
| ------------- | :----: | ----------------------------------------------- |
| `title`       |   O    | email subject. (dflt: `mqttwarn notification`)  |