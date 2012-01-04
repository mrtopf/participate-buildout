========
Themeing
========

We have the following situation:

- the usermanager has some templates
- participate has some templates
- participate has a master template
- participate has CSS and JS
- usermanager has JS, e.g. for validation
- participate and usermanager have different images, e.g. logos
- participate and usermanager are different servers and thus have different JS and CSS and images

- We might want to have a themes product which changes CSS, JS and optionally all of the templates, esp. the master template
- necessary JS imports might be a macro and could be reused 

How is everything configured?

Example:

- participate and usermanager use the same or similar master macros
- both have different template prefixes. One for a maybe shared master and one for work templates

Template definition via python
==============================

Simply create a ``config.py`` script per server which contains a function like that::
    
    def update_settings(settings):
        settings.templates = ...

In this config file you can update anything in the settings. It will be called after all initialization is done.


How to configure which config file to use?
------------------------------------------

In order to do that 



Templates
=========




Including blocks
================

The usermanager has no menu structure in the beginning because that's part of the host app. Nevertheless the host app
might want to integrate some blocks of content such as a menu. For the master template this can be done by overriding the
master template. And that's maybe enough for now. 

.. warning:: 
    Find a way to include custom blocks in a template. They might be passed in as variables with pre-rendered content or
    in form of some context menu.



