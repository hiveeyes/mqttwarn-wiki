The `redispub` plugin publishes to a Redis channel.

```ini
[config:redispub]
host  =  'localhost'
port  =  6379
targets = {
    'r1'      : [ 'channel-1' ]
    }
```

Requires:
* Python [redis-py](https://github.com/andymccurdy/redis-py)