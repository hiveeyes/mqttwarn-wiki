To _warn_, _alert_, or _notify_.

![Definition by Google](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/google-definition.jpg)

This program subscribes to any number of MQTT topics (which may include wildcards) and publishes received payloads to one or more notification services, including support for notifying more than one distinct service for the same message.

For example, you may wish to notify via [[e-mail|smtp]] and to [[Pushover]] of an alarm published as text to the MQTT topic `home/monitoring/+`.

![Overview](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/mqttwarn.png)

| #     | Plugins                                     |
| :---: |:--------------------------------------------|
| **a** | [apns](apns) |
| **c** | [carbon](carbon) |
| **d** | [dbus](dbus), [dnsupdate](dnsupdate) |
| **e** | [emoncms](emoncms) |
| **f** | [file](file), [freeswitch](freeswitch) |
| **g** | [gss](gss) |
| **h** | [http](http) |
| **i** | [instapush](instapush), [irccat](irccat) |
| **l** | [linuxnotify](linuxnotify), [log](log) |
| **m** | [mqtt](mqtt), [mqttpub](mqttpub), [mysql](mysql), [mysql_dynamic](mysql_dynamic), [mythtv](mythtv) |
| **n** | [nma](nma), [nntp](nntp), [nsca](nsca) |
| **o** | [osxnotify](osxnotify), [osxsay](osxsay) |
| **p** | [pastebinpub](pastebinpub), [pipe](pipe), [prowl](prowl), [pushalot](pushalot), [pushbullet](pushbullet), [pushover](pushover) |
| **r** | [redispub](redispub) |
| **s** | [slack](slack), [smtp](smtp), [sqlite](sqlite), [syslog](syslog) |
| **t** | [twilio](twilio), [twitter](twitter) |
| **x** | [xbmc](xbmc), [xmpp](xmpp), [xively](xively) |
| **z** | [zabbix](zabbix) |

Notifications are transmitted to the appropriate service via [[plugins]]. We provide plugins for the above list of services, and you can easily [[add your own|Plugins]].

[Jan-Piet Mens](http://jpmens.net), the main developer, wrote an introductory post, explaining [what mqttwarn can be used for](http://jpmens.net/2014/04/03/how-do-your-servers-talk-to-you/).

* [[Setup]]
* [[Getting started]]
* [[The configuration file|Configuration of service plugins]]
* [[Transformation]]
* [[Configuration of service plugins]]
* [[Plugins]]
* [[Advanced features]]
* [[Examples]]

## Press

* [MQTTwarn: Ein Rundum-Sorglos-Notifier](http://jaxenter.de/news/MQTTwarn-Ein-Rundum-Sorglos-Notifier-171312), article in German at JAXenter.