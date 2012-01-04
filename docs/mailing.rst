================================
Mailing Component: postverteiler
================================

Mailing ist done via it's own service. It's running with a simple RESTful web API. 
It's secured by a two-legged OAuth process so that both server and client need to 
exchange secrets beforehand.

Templates
=========

Templates are stored inside the web server. They can either be stored via configuration
or they can be updated and replaced via the API (in a future version at least).

Sending Mail
============

For sending an email the service needs the following information:

* the id of the template to use (``TEMPLATE``)
* the recipient (email and name) (``RECIPIENT_NAME`` and ``RECIPIENT_EMAIL``)
* the sender (might come from configuration) (``SENDER_NAME`` and ``SENDER_EMAIL``)
* the subject (also subject to replacements as ``SUBJECT``)
* variables to be replaced inside the template

Sending email is done via POST and all data is sent as form-encoded values.

The server will return a status code like ``200 OK`` for a successful mail
and ``400 Bad Request`` for a not successful email. 

TODO: We might need some status service to check for bounces etc. Maybe callbacks would be good as well.

Dummy Mail
==========

For debugging and testing purposes the server can be configured to not send email but instead
store the emails temporarily with a service to read them again.



Mail API
========

The service can also be used without running a server by simply calling the mail API directly::

    from postverteiler import mailapi

    server = smptlib.SMTP() # the actual sending endpoint
    mailer = mailapi.MailAPI(server, encoding="utf-8", from_addr='"Somebody" <email@example.org>')
    mailer.set_template_loader(PackageLoader("package","email_templates"))

    params = {
        "invitation_code": "2226827628762";
    }

    mailer.mail("Example User", "foo@example.org", "welcome.txt", params)

