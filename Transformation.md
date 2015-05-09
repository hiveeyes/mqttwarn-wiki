In addition to passing the [[payload received|Replacement]] via MQTT to a service, _mqttwarn_ allows you do do the following:

* Transform payloads on a per/topic basis. For example, you know you'll be receiving JSON, but you want to warn with a nicely formatted message.
* For certain services, you can change the _title_ (or _subject_) of the outgoing message.
* For certain services, you can change the _priority_ of the outgoing message.

Consider the following JSON payload published to the MQTT broker:

```shell
mosquitto_pub -t 'osx/json' -m '{"fruit":"banana", "price": 63, "tst" : "1391779336"}'
```

Using `format` we can configure _mqttwarn_ to transform that JSON into a different outgoing message which is the text that is actually notified. Part of said `format` looks like this in the configuration file, and basically specifies that messages published to `osx/json` should be transformed as on the right-hand side.

```ini
format = "I'll have a {fruit} if it costs {price}"
```

(Embedded `"\n"` are converted to newlines on output.)

The result is:

![OSX notifier](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/jmbp-840.jpg)

You associate MQTT topic branches to applications in the configuration file (copy `mqttwarn.ini.sample` to `mqttwarn.ini` for use). In other words, you can accomplish, say, following mappings:

* PUBs to `owntracks/jane/iphone` should be notified via Pushover to John's phone
* PUBs to `openhab/temperature` should be Tweeted
* PUBs to `home/monitoring/alert/+` should notify Twitter, Mail, and Prowl

See details in the config sample for how to configure this script.
The path to the configuration file (which must be valid Python) is obtained from the `MQTTWARNINI` environment variable which defaults to `mqttwarn.ini` in the current directory.