The `slack` plugin posts messages to channels in or users of the [slack.com](http://slack.com) service. The configuration of this service requires an API token obtaininable there.

```ini
[config:slack]
token = 'xxxx-1234567890-1234567890-1234567890-1234a1'
targets = {
              #  #channel/@user   username, icon
   'jpmens'  : [ '@jpmens',       "Alerter",   ':door:' ],
   'general'  : [ '#general',     "mqttwarn",   ':syringe:' ],
  }
```

Each target defines the name of an existing channel (`#channelname`) or a user (`@username`) to be
addressed, the name of the sending user as well as an [emoji icon](http://www.emoji-cheat-sheet.com) to use.

![slack](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/slack.png)

This plugin requires [Python slacker](https://github.com/os/slacker).