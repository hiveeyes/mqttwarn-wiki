The `linuxnotify` service is used to display notifications on a running desktop environment.

```ini
[config:linuxnotify]
targets = {
    'warn' : [ 'Warning' ]
    }
```

![linuxnotify](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/linuxnotify.png)

Requires:
* Gnome3 as desktop environment
* gobject-introspection Python bindings