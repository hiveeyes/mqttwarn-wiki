The `rrdtool` plugin updates a round robin database created by [rrdtool](http://oss.oetiker.ch/rrdtool/) with the message payload.

```ini
[config:rrdtool]
targets = {
    'living-temp'  : ['/tmp/living-temp.rrd',  '--template', 'temp'],
    'kitchen-temp' : ['/tmp/kitchen-temp.rrd', '--template', 'temp']
    }
```

[rrdpython's API](http://oss.oetiker.ch/rrdtool/prog/rrdpython.en.html) expects strings and/or list of strings as parameters to the functions. Thus a list for a target simply contains the command line arguments for `rrdtool update`. The plugin will embed the message as final argument `N:<message>`.

Requires the rrdtool bindings available with `pip install rrdtool`.