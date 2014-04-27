The `pipe` target launches the specified program and its arguments and pipes the (possibly formatted) message to the program's _stdin_. If the message doesn't have a training newline (`\n`), _mqttwarn_ appends one.

```ini
[config:pipe]
targets = {
             # argv0 .....
   'wc'    : [ 'wc',   '-l' ]
   }
```

Note, that for each message targeted to the `pipe` service, a new process is spawned (fork/exec), so it is quite "expensive".