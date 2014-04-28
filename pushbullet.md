This service is for [PushBullet](https://www.pushbullet.com), an app for Android along with extensions for Chrome and Firefox, which allows notes, links, pictures, addresses and files to be sent between devices.

You can get your API key from [PushBullet account](https://www.pushbullet.com/account) after signing up. You will also need the device ID to push the notifications to. To obtain this, you need to follow the instructions at [pyPushBullet](https://github.com/Azelphur/pyPushBullet) and run ``./pushbullet_cmd.py YOUR_API_KEY_HERE getdevices``.

```ini
[config:pushbullet]
targets = {
                   # API KEY                  device ID
    'warnme'   : [ 'xxxxxxxxxxxxxxxxxxxxxxx', 'yyyyyy' ]
    }
```

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `title`       |   O    | application title (dflt: `mqttwarn`)   |

![Pushbullet](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/pushbullet.jpg)

Requires:
* a [Pushbullet](https://www.pushbullet.com) account with API key
* [pyPushBullet](https://github.com/Azelphur/pyPushBullet). You don't have to install this -- simply copy `pushbullet.py` to the _mqttwarn_ directory.