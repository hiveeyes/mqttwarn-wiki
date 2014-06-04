The `zabbix` service serves two purposes:

1. it can create a [Zabbix] host on-the-fly via Low-level Discovery (LLD)
2. it can send an item/value pair to a [Zabbix] trapper

![Zabbix](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/zabbix.png)
![Zabbix](assets/zabbix.png)

The target and topic configuration look like this:

```ini
[config:zabbix]
targets = {
            # Trapper address   port
    't1'  : [ '172.16.153.110', 10051 ],
  }

[zabbix/clients/+]
alldata = ZabbixData()
targets = zabbix:t1

[zabbix/item/#]
alldata = ZabbixData()
targets = zabbix:t1
```

A transformation function in `alldata` is required to extract the client's name
from the topic, and for #1, to define a "host alive" item key in [Zabbix].

```python
# If the topic begins with zabbix/clients we have a host going up or down
# e.g. "zabbix/clients/jog03" -> "jog03"
#   extract client name (3rd part of topic)
#   set status key (e.g. 'host.up') to publish 1/0 on it (e.g during LWT)
#
# if the topic starts with zabbix/item we have an item/value for the host
# e.g. "zabbix/item/jog03/time.stamp" -> "jog03"
#   extract client name (3rd part of topic)
#

def ZabbixData(topic, data, srv=None):
    client = 'unknown'
    key = None
    status_key = None

    parts = topic.split('/')
    client = parts[2]

    if topic.startswith('zabbix/clients/'):
        status_key = 'host.up'

    if topic.startswith('zabbix/item/'):
        key = parts[3]

    return dict(client=client, key=key, status_key=status_key)
```

The blog post [Zabbix meets MQTT](http://jpmens.net/2014/05/27/zabbix-meets-mqtt/) explains a bit of the background.

  [Zabbix]: http://zabbix.com
