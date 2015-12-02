The `http` service allows GET and POST requests to an HTTP service.

Each target has five parameters:

1. The HTTP method (one of `get` or `post`)
2. The URL, which is transformed if possible (transformation errors are ignored)
3. `None` or a dict of parameters. Each individual parameter value is transformed.
4. `None` or a list of username/password e.g. `( 'username', 'password')`
5. `None` or True to force the transformation of the third parameter to a json object

```ini
[config:http]
timeout = 60

targets = {
                #method     #URL               # query params or None                              # list auth #Json
  'get1'    : [ "get",  "http://example.org?", { 'q': '{name}', 'isod' : '{_dtiso}', 'xx': 'yy' }, ('username', 'password') ],
  'post1    : [ "post", "http://example.net", { 'q': '{name}', 'isod' : '{_dtiso}', 'xx': 'yy' }, None, True ]
  }
```

Note that transforms in parameters must be quoted strings:

* Wrong: `'q' : {name}`
* Correct: `'q' : '{name}'`