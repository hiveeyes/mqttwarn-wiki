The `sqlite` plugin creates a table in the database file specified in the targets, and creates a schema with a single column called `payload` of type `TEXT`. _mqttwarn_ commits messages routed to such a target immediately.

```ini
[config:sqlite]
targets = {
                   #path        #tablename
  'demotable' : [ '/tmp/m.db',  'mqttwarn'  ]
  }
```
