The `file` service can be used for logging incoming topics, archiving, etc. Each message is written to a path specified in the targets list. Note that files are opened for appending and then closed on each notification.

Supposing we wish to archive all incoming messages to the branch `arch/#` to a file `/data/arch`, we could configure the following:

```ini
[config:file]
append_newline = True
targets = {
    'log-me'    : ['/data/arch']
   }
```
