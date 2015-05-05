The `dnsupdate` service updates an authoritative DNS server via RFC 2136 DNS Updates.
Consider the following configuration:

```ini
[config:dnsupdate]
dns_nameserver = '127.0.0.2'
dns_keyname= 'mqttwarn-auth'
dns_keyblob= 'kQNwTJ ... evi2DqP5UA=='
targets = {
   #target             DNS-Zone      DNS domain              TTL,  type
   'temp'         :  [ 'foo.aa.',     'temperature.foo.aa.', 300, 'TXT'   ],
   'addr'         :  [ 'foo.aa.',     'www.foo.aa.',         60,  'A'   ],
  }

[test/temp]
targets = log:info, dnsupdate:temp
format = Current temperature: {payload}C

[test/a]
targets = log:info, dnsupdate:addr
format = {payload}
```

`dns_nameserver` is the address of the authoritative server the update should be sent
to via a TCP update. `dns_keyname` and `dns_keyblob` are the TSIG key names and base64-representation of the key respectively. These can be created with either of:

```
ldns-keygen  -a hmac-sha256 -b 256 keyname
dnssec-keygen -n HOST -a HMAC-SHA256 -b 256 keyname
```

where _keyname_ is the name then added to `dns_keyname` (in this example: `mqttwarn-auth`).

Supposing a BIND DNS server configured to allow updates, you would then configure it
as follows:

```
key "mqttwarn-auth" {
  algorithm hmac-sha256;
  secret "kQNwTJ ... evi2DqP5UA==";
};

...
zone "foo.aa" in {
   type master;
   file "keytest/foo.aa";
   update-policy {
      grant mqttwarn-auth. zonesub ANY;
   };
};
```

For the `test/temp` topic, a pub and the resulting DNS query:

```
$ mosquitto_pub -t test/temp -m 42'
$ dig @127.0.0.2 +noall +answer temperature.foo.aa txt
temperature.foo.aa. 300 IN  TXT "Current temperature: 42C"
```

The `test/a` topic expects an address:

```
$ mosquitto_pub -t test/a -m 172.16.153.44
$ dig @127.0.0.2 +short www.foo.aa
172.16.153.44
```

Ensure you watch both mqttwarn's logfile as well as the log of your
authoritative name server which will show you what's going on:

```
client 127.0.0.2#52786/key mqttwarn-auth: view internal: updating zone 'foo.aa/IN': adding an RR at 'www.foo.aa' A 172.16.153.44
```

Requires:
* [dnspython](http://www.dnspython.org)