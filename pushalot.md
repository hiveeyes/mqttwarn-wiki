This service is for [pushalot](http://www.pushalot.com), which is a notifier app for Windows Phone and Windows 8.

It requires an Authorization token, which you can generate after creating an account at [pushalot.com](http://www.pushalot.com) We can then use that to configure the target definition:

```ini
[config:pushalot]
targets = {
                   # Authorization token
    'info'     : ['xxxxxxxxxxxxxxxxxxxxxxx'],
    'warn'     : ['xxxxxxxxxxxxxxxxxxxxxxx']
    }
````

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `title`       |   O    | application title (dflt: `mqttwarn`)   |

![Pushalot](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/pushalot.png)

Requires:
* a [pushalot](http://www.pushalot.com) account with Authorization token