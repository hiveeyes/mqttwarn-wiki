This service is for [Prowl](http://www.prowlapp.com). Each target requires an application key and an application name.

```ini
[config:prowl]
targets = {
                    # application key                           # app name
    'pjpm'    :  [ 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx', 'SuperAPP' ]
    }
```

| Topic option  |  M/O   | Description                            |
| ------------- | :----: | -------------------------------------- |
| `title`       |   O    | application title (dflt: `mqttwarn`)   |
| `priority`    |   O    | priority. (dflt: 0)                    |


![Prowl](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/prowl.jpg)

Requires:
* [prowlpy](https://github.com/jacobb/prowlpy)