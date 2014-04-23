An MQTT topic branch name contains information you may want to use in transformations. As a rather extreme example, consider the [OwnTracks] program (the artist formerly known as _MQTTitude_).

When an [OwnTracks] device detects a change of a configured waypoint or geo-fence (a region monitoring a user can set up on the device), it emits a JSON payload which looks like this, on a topic name consisting of `owntracks/_username_/_deviceid_`:

```
owntracks/jane/phone -m '{"_type": "location", "lat": "52.4770352" ..  "desc": "Home", "event": "leave"}'
```

In order to be able to obtain the username (`jane`) and her device name (`phone`) for use in transformations (see previous section), we would ideally want to parse the MQTT topic name and add that to the item data our plugins obtain. Yes, we can.

An optional `topicdatamap` in our configuration file, defines the name of a function we provide, also in the configuration file, which accomplishes that.

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
format = {username}: {event} => {desc}
```

the above PUBlish will be transformed into

```
jane: leave => Home
```