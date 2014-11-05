The `apns` service interacts with the Apple Push Notification Service (APNS) and
is a bit special (and one of _mqttwarn_'s more complex services) in as much as
it requires an X.509 certificate and a key which are typically available to
developers only.

The following discussion assumes one of these payloads published via MQTT:

```json
{"alert": "Vehicle moved" }
```

```json
{"alert": "Vehicle moved", "custom" : { "tid": "C2" }}
```

In both cases, the message which will be displayed in the notification of the iOS
device is "Vehicle moved". The second example depends on the app which receives
the notification. This custom data is per/app. This example app uses the custom
data to show a button:

![APNS notification](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/apns.png)

This is the configuration we'll discuss.

```ini
[defaults]
hostname  = 'localhost'
port      = 1883
functions = 'myfuncs'

launch	 = apns

[config:apns]
targets = {
                 # path to cert in PEM format   # key in PEM format
    'prod'     : ['/path/to/prod.crt',          '/path/to/prod.key'],
    }
    
[test/token/+]
targets = apns
alldata = apnsdata()
format  = {alert}
```

Certificate and Key files are in PEM format, and the key file must *not* be
password-protected. (The PKCS#12 file you get as a developer can be extracted thusly:

```
openssl pkcs12 -in apns-CTRL.p12 -nocerts -nodes | openssl rsa > prod.key
openssl pkcs12 -in apns-CTRL.p12 -clcerts -nokeys  > xxxx
```

then copy/paste from `xxxx` the sandbox or production certificate into `prod.crt`.)

The _myfuncs_ function `apnsdata()` extracts the last part of the topic into
`apns_token`, the hex token for the target device, which is required within the
`apns` service.

```python
def apnsdata(topic, data, srv=None):
    return dict(apns_token = topic.split('/')[-1])
```

A publish to topic `test/token/380757b117f15a46dff2bd0be1d57929c34124dacb28d346dedb14d3464325e5`
would thus emit the APNS notification to the specified device.


Requires:
*  [PyAPNs](https://github.com/djacobs/PyAPNs)