The `twilio` service makes use of the [Twilio](http://www.twilio.com/) API to send messages.

```ini
[config:twilio]
targets = {
             # Account SID            Auth Token            from              to
   'hola'  : [ 'ACXXXXXXXXXXXXXXXXX', 'YYYYYYYYYYYYYYYYYY', "+15105551234",  "+12125551234" ]
   }
```

![Twilio](https://raw.githubusercontent.com/jpmens/mqttwarn/master/assets/twillio.jpg)

Requires:
 * A [Twilio](http://www.twilio.com/) account
 * [twilio-python](https://github.com/twilio/twilio-python)