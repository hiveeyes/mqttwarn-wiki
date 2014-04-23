Instead of formatting output with the `format` specification as described above, _mqttwarn_ has provision for rendering the output message from [Jinja2](http://jinja.pocoo.org/docs/templates/) templates, probably particularly interesting for the `smtp` or `nntp` and `file` targets.

Consider the following example topic configuration, where we illustrate using a template instead of `format` (which is commented out).

```ini
[nn/+]
targets = nntp:jpaa
; format = {name}: {number} => {_dthhmm}
template = demo.j2
```

_mqttwarn_ loads Jinja2 templates from the `templates/` directory relative to the configured `directory`. Assuming we have the following content in the file `templates/demo.j2`

```jinja2
{#
    this is a comment
    in Jinja2
    See http://jinja.pocoo.org/docs/templates/ for information
    on Jinja2 templates.
#}
{% set upname = name | upper %}
{% set width = 60 %}
{% for n in range(0, width) %}-{% endfor %}

Name.................: {{ upname }}
Number...............: {{ number }}
Timestamp............: {{ _dthhmm }}
Original payload.....: {{ payload }}
```

could produce the following message, on any target which uses this configuration.

```
------------------------------------------------------------
Name.................: JANE JOLIE
Number...............: 47
Timestamp............: 19:15
Original payload.....: {"name":"Jane Jolie","number":47, "id":91}
```

One of the template variables you may be interested in is called `{{ payload }}`; this carries the original MQTT message in it. Also, if the payload was JSON, those are available also (as shown in the above example), together with all the other transformation data.

If the template cannot be rendered, say, it contains a Jinja2 error or the template file cannot be found, etc., the original raw message is used in lieu on output.

As mentioned already, we think this is useful for targets which expect a certain amount of text (`file`, `smtp`, and `nntp` come to mind).

Use of this feature requires [Jinja2](http://jinja.pocoo.org/docs/templates/), but you don't have to install it if you don't need
templating.
