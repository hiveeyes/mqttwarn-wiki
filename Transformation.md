_mqttwarn_ can transform an incoming message before passing it to a plugin service. On each message, _mqttwarn_ attempts to decode the incoming payload from JSON. If this is possible, a dict with _transformation data_ is made available to the service plugins as `item.data`.

This transformation data is initially set up with some built-in values, in addition to those decoded from the incoming JSON payload. The following built-in variables are defined in `item.data`:

```python
{
  'topic'         : topic name
  'payload'       : topic payload
  '_dtepoch'      : epoch time                  # 1392628581
  '_dtiso']       : ISO date (UTC)              # 2014-02-17T10:38:43.910691Z
  '_dthhmm'       : timestamp HH:MM (local)     # 10:16
  '_dthhmmss'     : timestamp HH:MM:SS (local)  # 10:16:21
}
```

Any of these values can be used in `format` to create custom outgoing messages.

```ini
format = I'll have a {fruit} if it costs {price} at {_dthhmm}
```