Notification of one or more [Twitter](http://twitter.com) accounts requires setting up an application at [apps.twitter.com](https://apps.twitter.com). For each Twitter account, you need four (4) bits which are named as shown below.

Upon configuring this service's targets, make sure the four (4) elements of the list are in the order specified!

```ini
[config:twitter]
targets = {
  'janejol'   :  [ 'vvvvvvvvvvvvvvvvvvvvvv',                              # consumer_key
                   'wwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwww',          # consumer_secret
                   'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',  # access_token_key
                   'zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz'           # access_token_secret
                  ]
   }
```

![A tweet](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/twitter.jpg)

Requires:

* A [Twitter](http://twitter.com) account
* Aapp keys for Twitter, from [apps.twitter.com](https://apps.twitter.com)
* [python-twitter](https://github.com/bear/python-twitter)