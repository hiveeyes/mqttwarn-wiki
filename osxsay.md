The `osxsay` target alerts you on your Mac (warning: requires a Mac :-) with a spoken voice. It pipes the message (which is hopefully text only) to the _say(1)_ utility. You can configure any number of different targets, each with a different voice (See `say -v ?` for a list of allowed voice names.)

```ini
[config:osxsay]
targets = {
                 # voice (see say(1) or `say -v ?`)
    'victoria' : [ 'Victoria' ],
    'alex'     : [ 'Alex' ],
  }
```

```ini
[say/warn]
targets = osxsay:victoria
```

```ini
[say/alert]
targets = osxsay:alex
```

* Note: this requires your speakers be enabled and can be a pain for co-workers or family members, and we can't show you a screen shot.