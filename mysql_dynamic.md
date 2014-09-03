Similar to the MySQL plugin but tables and columns are created dynamically as needed. The name of the table is composed from the topic, replacing the dash separator with underscores. As an example, the topic ```device/laptop/tracks```results in the creation of a table named ```device_laptop_tracks```.

The message will be processed and each JSON field will be stored in a different column. The columns of each table (and the table itself) are created when the first message is published to the topic. The configuration allows to specify the fields to ignore. These will not be stored in the database.

As an example, by publishing this JSON payload:
```
mosquitto_pub -t my/2 -m '{ "name" : "Jane Jolie", "id" : 90, "number" : 17 }'
```

A table named ```my_2``` will be created on the fly with the following structure and content (the table name is derived from the MQTT topic, but slashes are replaced by underscores):

```mysql
+------+------------+--------+-------------------------------------------------------+
| id   | name       | number | payload                                               |
+------+------------+--------+-------------------------------------------------------+
|   90 | Jane Jolie | 17     | '{ "name" : "Jane Jolie", "id" : 90, "number" : 17 }' |
+------+------------+--------+-------------------------------------------------------+
```
Please note that by default, the information is always stored in a duplicated form: each field, and all fields together as sent. If you can use the field ignore capability (see below) to disable this behaviour. Actually, lots of other fields (created by mqttwarn) may be present. Adjust your configuration as required.

An index table, containing a timestamp and the name of the topic, will keep track of the latest update to the remaining tables. The name of the index table can be specified in the configuration, and must be created manually. The following statements create an index table named ```index_table_name``:

```
CREATE TABLE `index_table_name` (
  `topic` text NOT NULL,
  `ts` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY `topic` (`topic`(128))
);
```

This module requires the following configuration to be present in the configuration file:

```ini
[config:mysql_dynamic]
host  =  'localhost'
port  =  3306
user  =  'dbusername'
pass  =  'dbpassword'
dbname  =  'database'
index   =  'index_table_name'

targets = {
        # target to use: [ list of fields to ignore and not store ]
        'target_name' : ['field1', 'field2','field3' ]
    }
```

Requires:
* [MySQLDb](http://mysql-python.sourceforge.net/)

Limitations:

At this point, if the payload format changes, the tables are not modified and data may fail to be stored. Also, there is no fallback table or column like the case of the MySQL plugin.