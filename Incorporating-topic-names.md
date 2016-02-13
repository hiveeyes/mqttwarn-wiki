## Intro

A MQTT topic name contains information you may want to use in transformation data.
After having the information decoded into transformation data, it can be reused
in `format` or topic `targets` directives.

Let's have a look into the details by means of two examples.


## Incorporate topic names into outgoing messages

As a rather extreme example, consider the [OwnTracks] program (the artist formerly known as _MQTTitude_).

When an [OwnTracks] device detects a change of a configured waypoint or geo-fence (a region monitoring a user can set up on the device), it emits a JSON payload which looks like this, on a topic name consisting of `owntracks/_username_/_deviceid_`:

```
owntracks/jane/phone -m '{"_type": "location", "lat": "52.4770352" ..  "desc": "Home", "event": "leave"}'
```

In order to be able to obtain the username (`jane`) and her device name (`phone`) for use in transformations (see previous section), we would ideally want to parse the MQTT topic name and add that to the item data our plugins obtain. Yes, we can.

An optional `datamap` in our configuration file, defines the name of a function we provide, also in the configuration file, which accomplishes that.

```ini
[owntracks/jane/phone]
datamap = OwnTracksTopicDataMap()
```

This specifies that when a message for the defined topic `owntracks/jane/phone` is processed, our function `OwnTracksTopicDataMap()` should be invoked to parse that. (As usual, topic names may contain MQTT wildcards.)

The function we define to do that is:

```python
def OwnTracksTopicDataMap(topic):
    if type(topic) == str:
        try:
            # owntracks/username/device
            parts = topic.split('/')
            username = parts[1]
            deviceid = parts[2]
        except:
            deviceid = 'unknown'
            username = 'unknown'
        return dict(username=username, device=deviceid)
    return None
```

The returned _dict_ is merged into the transformation data, i.e. it is made available to plugins and to transformation rules (`format`). If we then create the following rule

```ini
format = "{username}: {event} => {desc}"
```

the above PUBlish will be transformed into

```
jane: leave => Home
```



## Incorporate topic names into topic targets


### Goal
In another example we use information from the MQTT topic path to dynamically
resolve topic targets in order to dispatch messages to specified receivers.


### Decode topic names into transformation data
The [Hiveeyes bee monitoring system](https://github.com/hiveeyes) uses MQTT topics like
`{realm}/{network}/{gateway}/{node}/{field}` for transmitting sensor measurement values.
An example message looks like:

    hiveeyes/0ef-917-40b-a4-5b5/8sf83id9/1/temp1 22.5

Similar to the _OwnTracks_ scenario, we first need to split the topic path into segments
and put them into a dictionary. We take a slightly different approach and decode the
MQTT topic using regular expressions by defining a `datamap` function `hiveeyes_topic_to_topology`:

```python
import re

def hiveeyes_topic_to_topology(topic):
    if type(topic) == str:
        try:
            pattern = r'^(?P<realm>.+?)/(?P<network>.+?)/(?P<gateway>.+?)/(?P<node>.+?)/(?P<field>.+?)$'
            p = re.compile(pattern)
            m = p.match(topic)
            topology = m.groupdict()
        except:
            topology = {}
        return topology
    return None
```

Let's hook this function into the `mqttwarn.ini` configuration file:
```ini
[hiveeyes/#]
datamap = hiveeyes_topic_to_topology()
```

When receiving appropriate messages, we have the topic segments accessible inside the transformation data dictionary:

```json
{
    "realm":   "hiveeyes",
    "network": "0ef-917-40b-a4-5b5",
    "gateway": "8sf83id9",
    "node":    "1",
    "field":   "temp1"
}
```


### Format an appropriate outgoing message

The `format` configuration directive
```ini
format = "{field} of {gateway}_{node}@{network} is {payload}°C"
```

provides a nicely formatted, human readable message:

    temp1 of 8sf83id9_1@0ef-917-40b-a4-5b5 is 22.5°C


### Dispatch message to appropriate receivers

To recap, the goal is to inform members of our Beekeepersclub of
measurement values by dispatching messages to registered addresses.

Let's use `xmpp` by defining the following addressbook:
```ini
[config:xmpp]
sender   = 'hiveeyes@xmpp.beekeepersclub.org'
password = 'yourcatsname'
targets  = {
                '0ef-917-40b-a4-5b5' : ['research@xmpp.beekeepersclub.org'],
                '8sf83id9'           : ['berlin@xmpp.beekeepersclub.org'],
                '8sf83id9-1'         : ['peng@xmpp.beekeepersclub.org']
           }
```

In this example, the researchers of the Beekeepersclub would get informed about
every message on the network, while the Berlin section would only get messages
from a specific gateway. Peng, who is responsible for a single beehive,
would only get measurement data from a specific node.


By using a configuration like:
```ini
[hiveeyes/#]
datamap = hiveeyes_topic_to_topology()
targets = log:info, xmpp:{network}, xmpp:{gateway}, xmpp:{gateway}-{node}
```

this allows us to make _mqttwarn_ compute the right hand side of the designated
topic target dynamically based on information from the transformation data,
while only having to define a single service section in the configuration file.

Otherwise, we would have to define a service section for each and every
MQTT topic we want to monitor and dispatch messages from.
