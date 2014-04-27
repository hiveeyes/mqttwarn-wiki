The `freeswitch` service will make a VOIP call to the number specified in your target and 'speak' the message using the [Google Translate API](translate.google.com). Each target includes the gateway to use as well as the number/extension to call, so you can make internal calls direct to an extension, or call any external number using your external gateway.

In order to use this service you must enable the XML RPC API in Freeswitch - see instructions [here](http://wiki.freeswitch.org/wiki/Mod_xml_rpc).

```ini
[config:freeswitch]
host     = 'localhost'
port     = 8050
username = 'freeswitch'
password = '<xml_rpc_password>'
targets  = {
    'mobile'    : ['sofia/gateway/domain/', '0123456789']
    }
```

NOTE: only the first 100 chars of the message will be announced since this is the max length supported by the Google Translate API.

Requires
* [Freeswitch](https://www.freeswitch.org/)
* Internet connection for Google Translate API