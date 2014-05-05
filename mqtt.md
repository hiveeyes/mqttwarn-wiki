The `mqtt` service fires off a publish on a topic, creating a new connection to the configured broker for each message.

Consider the following configuration snippets:

```ini
[config:mqtt]
hostname =  'localhost'
port =  1883
qos =  0
retain =  False
username =  "jane"
password =  "secret"
targets = {
  'o1'    : [ 'out/food' ],
  'o2'    : [ 'out/fruit/{fruit}' ],
  'm2'    : [ 'sometopic',  'specialmq.ini' ],
  }

[in/a1]
targets = mqtt:o1, mqtt:o2
format =  u'Since when does a {fruit} cost {price}?'
```

The `topicmap` specifies we should subscribe to `in/a1` and republish to two MQTT targets. 
The second target (`mqtt:o2`) has a topic branch with a variable in it which is to be
interpolated (`{fruit}`).

These are the results for appropriate publishes:

```
$ mosquitto_pub -t 'in/a1' -m '{"fruit":"pineapple", "price": 131, "tst" : "1391779336"}'

in/a1 {"fruit":"pineapple", "price": 131, "tst" : "1391779336"}
out/food Since when does a pineapple cost 131?
out/fruit/pineapple Since when does a pineapple cost 131?


$ mosquitto_pub -t 'in/a1' -m 'temperature: 12'

in/a1 temperature: 12
out/food temperature: 12
out/fruit/{fruit} temperature: 12
```

In the first case, the JSON payload was decoded and the _fruit_ variable could be interpolated into the topic name of the outgoing publish, whereas the latter shows the outgoing topic branch without interpolated values, because they simply didn't exist in the original incoming payload.

In the first case, the JSON payload was decoded and the _fruit_ variable could be
interpolated into the topic name of the outgoing publish, whereas the latter shows
the outgoing topic branch without interpolated values, because they simply didn't
exist in the original incoming payload.

The optional second value in the topic map (`specialmq.ini` in the example above)
specifies the name of an INI-type file with parameters which override the basic
configuration of this service. Assume most of your MQTT targets go to `localhost`,
but you want one target to be configured to address a distinct MQTT broker. Create
an INI file with any name you desire and specify that as the optional second parameter:

```ini
[defaults]
hostname= 10.0.12.1
port= 1884
client_id = blub01
qos = 1
retain = False

[auth]
username = jjolie
password = seecret

[tls]
ca_certs = foobar.crt
;certfile = xxx.crt
;keyfile = xxx.key
tls_version = tlsv1
;ciphers = xxxxx xx
```

This shows the currently full configuration possible. Global values from the
`mqtt` service override those not specified here. Also, if you don't need
authentication (`auth`) or (`tls`) you may omit those sections. (The `defaults`
section must exist.)