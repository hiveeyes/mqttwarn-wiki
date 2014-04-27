The `nma` service uses [NMA (Notify My Android)](http://www.notifymyandroid.com) to delivery notifications from _mqttwarn_ to your Android device.

```ini
[config:nma]
targets = {
                 # api key                                            app         event
  'myapp'    : [ 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx', "Nagios",  "Phone call" ]
  }
```

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `priority`    |   O    | priority. (dflt: 0)                    |

![NMA](assets/nma.jpg)

Requires:
* A [Notify My Android(NMA)](http://www.notifymyandroid.com) account
* [pynma](https://github.com/uskr/pynma). You don't have to install this -- just copy `pynma.py` to the _mqttwarn_ directory.