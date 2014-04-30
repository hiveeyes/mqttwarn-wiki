This service is for [Pushover](https://pushover.net), an app for iOS and Android. In order to receive pushover notifications you need what is called a _user key_ and one or more _application keys_ which you configure in the targets definition:

```ini
[config:pushover]
targets = {
    'nagios'     : ['userkey1', 'appkey1'],
    'alerts'     : ['userkey2', 'appkey2'],
    'tracking'   : ['userkey1', 'appkey2'],
    'extraphone' : ['userkey2', 'appkey3']
    }
```

This defines four targets (`nagios`, `alerts`, etc.) which are directed to the configured _user key_ and _app key_ combinations. This in turn enables you to notify, say, one or more of your devices as well as one for your spouse.

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `title`       |   O    | application title (dflt: pushover dflt) |
| `priority`    |   O    | priority. (dflt: pushover setting)     |

![pushover on iOS](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/pushover.png)

Requires:
* A [pushover.net](https://pushover.net/) account