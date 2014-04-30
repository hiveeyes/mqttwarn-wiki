The `gss` service interacts directly with a Google Docs Spreadsheet. Each message can be written to a row in a selected worksheet.

Each target has two parameters:

1. The spreadsheet key. This is directly obtainable from the url of the open sheet.
2. The worksheet id. By default the first sheets id is 'od6'

```ini
[config:gss]
username    = your.username@gmail.com
password    = yourpassword
targets     = {
               # spreadsheet_key                               # worksheet_id
    'test': [ 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',  'od6']
    }
```

Example:

```
mosquitto_pub -t nn/ohoh -m '{"username": "jan", "device":"phone", "lat": "-33.8746097", "lon": "18.6292892", "batt": "94"}'
```

turns into

![GSS](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/gss.png)

Note: It is important that the top row into your blank spreadsheet has column headings that correspond the values that represent your dictionary keys.
If these column headers are not available, you will most likely see an error like this:

```
gdata.service.RequestError: {'status': 400, 'body': 'We&#39;re sorry, a server error occurred. Please wait a bit and try reloading your spreadsheet.', 'reason': 'Bad Request'}
```

Requires:
* [gdata-python-client](https://code.google.com/p/gdata-python-client/)