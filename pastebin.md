The `pastebinpub` service is publishing messages to [Pastebin](http://pastebin.com).

Note: Be careful what you post on this target, it could be public. If you are not a paying customer of Pastebin you are limited to 25 unlisted and 10 private pastes per 24 h.

```ini
[config:pastebinpub]
targets = {
    'warn' : [ 'api_dev_key',  # API dev key
               'username',  # Username
               'password',  # Password
                1,  # Privacy level
               '1H'  # Expire
            ]
    }
```

![osxnotify](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/pastebin.png)

Requires:
* An account at [Pastebin](http://pastebin.com)
* Python bindings for the [Pastebin API](https://github.com/Morrolan/PastebinAPI)
  You don't have to install this -- simply copy `pastebin.py` to the _mqttwarn_ directory.
  `curl -O https://raw.githubusercontent.com/Morrolan/PastebinAPI/master/pastebin.py`