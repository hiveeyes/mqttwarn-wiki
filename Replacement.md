Consider the following configuration snippet in addition to the configuration of the `mqtt` service shown above:

```python
def lookup_data(data):
    if type(data) == dict and 'fruit' in data:
            return "Ananas"
    return None
```

Then, in the section defining the topic we listen on:

```ini
...
[test/topic]
#format =  Since when does a {fruit} cost {price}?
format =  lookup_data()
```

We've replaced the `formatmap` entry for the topic by a function which you define within the _functions_ file you configure as `functions` in `mqttwarn.ini` configuration file. These functions are invoked with decoded JSON `data` passed to them as a _dict_. The string returned by the function returned string replaces the outgoing `message`:

```
in/a1 {"fruit":"pineapple", "price": 131, "tst" : "1391779336"}
out/food Ananas
out/fruit/pineapple Ananas
```

If a function operating on a message (i.e. within `format =`) returns `None` or an empty string, the target notification is suppressed.