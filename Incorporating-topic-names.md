## Intro

An MQTT topic branch name contains information you may want to use for payload transformations or _service targets_.


## Incorporation into payload transformations

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


## Incorporation into topic targets

In another example you may want to use the subscription topic path to distinguish different _service targets_. In the use case of [_hiveeyes_](https:swarm.hiveeyes.org) the topic
contains `{realm}/{network}/{gateway}/{node}/{kind}`.  For example:

```
hiveeyes/0ef-917-40b-a4-5b5/8sf83id9/1/temp1 22.5
```
As above, we first need to split the topic path into segments and put them into a dictionary.

```python
# this funtion goes to myfunc.py
import re

def hiveeyes_topic_to_topology(topic):
    if type(topic) == str:
        try:
            pattern = r'^(?P<realm>.+?)/(?P<network>.+?)/(?P<gateway>.+?)/(?P<node>.+?)(?:/(?P<kind>.+?))?$'
            p = re.compile(pattern)
	    m = p.match(topic)
            topology = m.groupdict()
        except:
            topology = {}
        return topology
    return None

```
The obtained _dict_ is accessible for other plugins.

We of course need to include the `myfunc.py` in `mqttwarn.ini` and call the above function with `datamap=` in the relevant subscription section.

```ini
#mqttwarn.ini

[defaults]
functions = 'myfuncs'
# ...
[hiveeyes/#]
datamap = hiveeyes_topic_to_topology()

```
So far we have the topic segments accessible for e.g. payload transformation as above:

```
format = "{kind} of {node}_{gateway}@{network} is {payload}°C"
```
would give us:
```
temp1 of 1_8sf83id9@0ef-917-40b-a4-5b5 is 22.5°C
```
But to incorporate the topic segments into potential _service targets_ we need to use a
function to compute the topic target list.

```python
# this function goes to myfunc.py as well
def TopicTargetList(topic=None, payload=None, data=None, srv=None):
    """
    Custom function to compute list of topic subscription
    targets based on topic and/or transformation data.
    Pass MQTT topic, transformation data, service object
    and optionally raw message payload.
    """
    if srv is not None:
        srv.logging.debug('topic={topic}, payload={payload}, data={data}, srv={srv}'.format(**locals()))

    # Use a fixed list of topic subscription targets for demonstration purposes.
    # In the real world, you would look up proper targets based on information
    # derived from transformation data, which in turn might have been enriched
    # by ``datamap`` or ``alldata`` transformation functions before.

    targets = ['log:info', 'xmpp:{network}', 'xmpp:{gateway}', 'xmpp:{node}']

```
While in this function we decide for static services it allows us to compute
the right hand side dynamical depending on e.g. the `{network}`.

We need to invoke our function under the subscription with `targets =
TopicTargetList()` and configure a service e.g. xmpp to use the choosen segment to identify a
user.

```ini
#mqttwarn.ini
# ...
[hiveeyes/#]
datamap = hiveeyes_topic_to_topology()
targets = TopicTargetList()
# ...
[config:xmpp]
sender = 'hiveeyes@xmpp.beekeepersclub.org'
password = 'yourcatsname'
targets = {
            '0ef-917-40b-a4-5b5' : ['research@xmpp.beekeepersclub.org'],
            '8sf83id9' : ['section_BER@xmpp.beekeepersclub.org'],
            '1' : ['peng@xmpp.beekeepersclub.org']
         }
```

In the example the researchers of the Beekeepersclub would get informed about every message on the network, while the BER section would only get messages from the BER gateway. Peng, who is responsible for a single hive, would only get data from this node. That way, you only have to define the service target once per service, but not for every single subscription.
