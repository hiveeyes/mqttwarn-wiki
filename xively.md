The `xively` service can send a subset of your data to [Xively](http://xively.com) per defined feedid.

```ini
[config:xively]
apikey = '1234567890abcdefghiklmnopqrstuvwxyz'
targets = {
        # feedid        : [ 'datastream1', 'datastream2'] 
        '1234567' : [ 'dataItem1', 'dataItem2' ],
        '7654321' : [ 'dataItemA' ]
  }
```

Requires:
* [Xively](http://xively.com) account with an already existing Feed
* [xively-python](https://github.com/xively/xively-python) - pip install xively-python