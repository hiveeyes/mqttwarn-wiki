This service is for [Instapush](https://instapush.im), an app for both IOS and Android, which provides free instant notifications.

You should first create an application and respective event following the [tutorial] (https://instapush.im/home/start/).

Afterward you will find your Application ID and Application Secret in the "Basic Info" of your application.

Each target corresponds to an event in your instapush application, you can define as many trackers as you wish as long as it's a JSON object.

| Field          | Value                |
| -------------- | -------------------- |
| `Event title`  | alerts               |
| `Trackers`     | object, action       |
| `Push Message` | {object} just {action} |

```ini
[config:instapush]
appid = '12345abc123456'
appsecret = '1234567890abcd123456789abcdef123456789'
targets = {
             # event   # trackers
  'notify' : [ 'alerts', {"object":"door", "action":"opened/closed"}]
  }
```

![instapush](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/instapush.png)