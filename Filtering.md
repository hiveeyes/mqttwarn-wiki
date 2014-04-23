A notification can be filtered (or supressed) using a custom function.

An optional `filter` in our configuration file, defines the name of a function we provide, also in the configuration file, which accomplishes that.

```ini
filter = owntracks_filter()
```

This specifies that when a message for the defined topic `owntracks/jane/phone` is processed, our function `owntracks_filter()` should be invoked to parse that. The filter function should return `True` if the message should be suppressed, or `False` if the message should be processed. (As usual, topic names may contain MQTT wildcards.)

The function we define to do that is:

```python
def owntracks_filter(topic, message):
    return message.find('event') == -1
```

This filter will suppress any messages that do not contain the `event` token.
