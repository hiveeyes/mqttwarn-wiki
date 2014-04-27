The `dbus` service send a message over the dbus to the user's desktop.

```ini
[config:dbus]
targets = {
    'warn' : [ 'Warning' ],
    'note' : [ 'Note' ]
    }
```

Requires:
* Python [dbus](http://www.freedesktop.org/wiki/Software/DBusBindings/#Python) bindings