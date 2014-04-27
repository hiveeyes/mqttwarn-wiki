To _warn_, _alert_, or _notify_.

![Definition by Google](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/jmbp-841.jpg)

This program subscribes to any number of MQTT topics (which may include wildcards) and publishes received payloads to one or more notification services, including support for notifying more than one distinct service for the same message.

For example, you may wish to notify via e-mail and to Pushover of an alarm published as text to the MQTT topic `home/monitoring/+`.

![Overview](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/mqttwarn.png)

| #     | Plugins                                     |
| :---: |:--------------------------------------------|
| **f** | [file](file), [freeswitch](freeswitch) |
| **g** | [gss](gss)  |
| **h** | [http](http) |
| **i** | [irccat](irccat) |
| **l** | [log](log) |
| **m** | [mqtt](mqtt), [mqttpub](mqttpub), [mysql](mysql) |
| **n** | [nma](nma), [nntp](nntp), [nsca](nsca) |
| **o** | [osxnotify](osxnotify), [osxsay](osxsay) |
| **p** | [pipe](pipe), [prowl](prowl), [pushbullet](pushbullet), [pushover](pushover) |
| **r** | [redispub](redispub) |
| **s** | [sqlite](sqlite), [smtp](smtp), [syslog](syslog), |
| **t** | [twilio](stwilio), [twitter](twitter) |
| **x** | [xbmc](xbmc) |

Notifications are transmitted to the appropriate service via plugins. We provide plugins for the above list of services, and you can easily add your [own](https://github.com/jpmens/mqttwarn/wiki/Plugins).

The developer wrote an introductory post, explaining [what mqttwarn can be used for](http://jpmens.net/2014/04/03/how-do-your-servers-talk-to-you/).

* [Setup](https://github.com/jpmens/mqttwarn/wiki/Setup)
* [Getting started](https://github.com/jpmens/mqttwarn/wiki/Getting-started)
* [The configuration file](https://github.com/jpmens/mqttwarn/wiki/Cofniguration-file)
* [Transformation](https://github.com/jpmens/mqttwarn/wiki/Transformation)
* [Configuration of service plugins](https://github.com/jpmens/mqttwarn/wiki/Configuration-of-service-plugins)
* [Plugins](https://github.com/jpmens/mqttwarn/wiki/Plugins)
* [Advanced features](https://github.com/jpmens/mqttwarn/wiki/Advanced-features)
* [Examples](https://github.com/jpmens/mqttwarn/wiki/Examples)

## Press

* [MQTTwarn: Ein Rundum-Sorglos-Notifier](http://jaxenter.de/news/MQTTwarn-Ein-Rundum-Sorglos-Notifier-171312), article in German at JAXenter.