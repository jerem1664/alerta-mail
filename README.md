###############
# alerta-mail #                                                                                                                                                                                                                                
###############


mailer for alerta (fork from https://github.com/alerta/alerta-contrib/tree/master/integrations/mailer) 



Alerts meet the following criteria:

  * must not be a duplicate alert (ie. ``repeat != True``)
  * must have status of ``open`` or ``closed``
  * must have a current severity *OR* previous severity of ``critical`` or ``major``
  * must not have been cleared down within 30 seconds (to prevent flapping alerts spamming)

To achieve the above, alerts are actually held for a minimum of 30 seconds
before they generate emails.

# Dependencies

The Alerta server *MUST* have the AMQP plugin enabled and configured.
(https://github.com/alerta/alerta-contrib/tree/master/plugins/amqp)

# Installation

Clone the GitHub repo and run:

    $ python setup.py install

# Configuration

See mailer.conf.example

and alertad.conf.example

# launch


    $ export SMTP_PASSWORD=okvqhitqomebufyv
    $ alerta-mailer


# Rules File

Notifications to other emails according regexp criteria can be enabled,
creating a JSON formatted file under ```alerta.rules.d/``` with the
following format:

```
[
    {
        "name": "foo",
        "fields": [
            {"field": "resource", "regex": "db-\w+"}
        ],
        "contacts": ["dba@lists.mycompany.com", "dev@lists.mycompany.com"]
    },
    {
        "name": "bar",
        "fields": [
            {"field": "resource", "regex": "web-\w+"}
        ],
        "contacts": ["dev@lists.mycompany.com"],
        "exclude" : true
    }
]
```

``field`` is a reference to the alert object, regex is a valid python
regexp and contacts are a list of mails who will receive an e-mail if
the regular expression matches.

Multiple ``field`` dictionary can be supplied and all ``regex`` must
match for the email to be sent.

If the ``exclude`` parameter is set, contact list will be cleared and
replaced with only the contacts of the current matched rule.

# Test

    $ python setup.py test
