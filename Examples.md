This section contains some examples of how `mqttwarn` can be used with some more complex configurations.

### Low battery notifications ###

By subscribing to your [OwnTracks] topic and adding the following custom filter you can get `mqttwarn` to send notifications when your phone battery gets below a certain level;

```python
def owntracks_battfilter(topic, message):
    data = dict(json.loads(message).items())
    if data['batt'] is not None:
        return int(data['batt']) > 20
    return True
```

Now simply add your choice of target(s) to the topic's section and a nice format string and you are done;

```ini
[owntracks/#]
targets = pushover, xbmc
filter = owntracks_battfilter()
format = My phone battery is getting low ({batt}%)!
```

### Producing JSON

Assuming we get, from an Arduino, say, a single numerical value in the payload of an MQTT
message, we want to generate JSON with some additional fields. Using a Jinja2 template
for the task, does exactly what we need:

The following target configuration invokes the template:

```ini
[arduino/temp]
targets = log:info, http:graylog2
template = temp2json.json
```

The Jinja2 template looks like this:

```jinja2
{#
    We expect a single numeric temperature value in `payload'
    Return JSON suitable for Graylog2 (requires host and short_message)

    Define a data structure in Jinja2 and return it as a JSON string.
    Note how transformation data (produced within mqttwarn) is used:
    the variables `_dtiso' and `payload' contain a timestamp and our
    payload respectively.
#}
{% set data = {
	'host'		: topic,
	'short_message'	: "Heat " + payload,
	'tst'		: _dtiso,
	'temperature'	: payload,
	'woohooo'	: 17,
	}
	%}
{{ data | jsonify }}
```

and an example JSON string returned by that template is then passed to our configured
targets thusly:

```json
"host": "arduino/temp", "woohooo": 17, "tst": "2014-04-13T09:25:46.247150Z", "temperature": "22", "short_message": "Heat 22"}
```
