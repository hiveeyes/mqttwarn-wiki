The `xmpp` service sends notification to one or more [XMPP (Jabber)](http://en.wikipedia.org/wiki/XMPP) recipients.

```ini
[config:xmpp]
sender = 'mqttwarn@jabber.server'
password = 'Password for sender'
targets = {
    'admin' : [ 'admin1@jabber.server', 'admin2@jabber.server' ]
    }
```

Targets may contain more than one recipient, in which case all specified recipients get the message.

Requires:
* XMPP (Jabber) accounts (at least one for the sender and one for the recipient)
* [xmpppy](http://xmpppy.sourceforge.net)