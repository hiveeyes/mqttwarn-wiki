dThe `log` service allows us to use the logging system in use by _mqttwarn_ proper, i.e. messages directed at `log` will land in _mqttwarn_'s log file.

```ini
[config:log]
targets = {
    'info'   : [ 'info' ],
    'warn'   : [ 'warn' ],
    'crit'   : [ 'crit' ],
    'error'  : [ 'error' ]
  }
```