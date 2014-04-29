Christian Hofstaedtler had a neat idea: forward IRC messages [with an awayforward.pl](https://gist.github.com/zeha/11387703) script to MQTT from which _mqttwarn_ can post them to a notification service.

Once it's loaded, `awayforward.pl` posts a JSON data structure to MQTT:

```json
{"target":"#channel","message":"<nick> good morning mynick"}
```

I can then create an _mqttwarn_ target which forwards those highlights to, say, _pushover_:

```ini
[irssi/away]
topic = irssi/away
targets = log:info, pushover:irssi
title = irssi: {target}
format = {message}
```
