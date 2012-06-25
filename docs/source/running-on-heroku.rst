Running on Heroku
=================

Normally, deploying a Django site would make you want to flip your desk:

.. image:: _static/flip.png

Luckily for us, `Heroku <http://www.heroku.com/>`_ has made the process a
complete joy! If you're aren't familiar with Heroku--they are the **best**
web host, and you will love them if you don't already.

``django-skel`` ships with a production ready Heroku configuration module, and
this section will walk you through creating your Heroku app, and getting your
site running in production.

While this section is quite long, don't be intimidated! It's only long because
I'm explaining everything along the way--the reality of it is that deploying
your site this way really only consists of a couple commands.

If you'd like to read some official documentation on the topic, check out
`Heroku's Django documentation <https://devcenter.heroku.com/articles/django>`_.


Step 1 - Create Your Heroku Application
---------------------------------------

The first step in getting your site running on Heroku is, as I'm sure you've
guessed, to create a Heroku app! Let's do it now::

    $ heroku create [your_app_name_here]

If you don't specify an app name, one will be automatically assigned to you. I
like to name my apps explicitly, because I have a bunch of them, and it's a lot
easier to track.

The next thing you'll need to do is push your project code to Heroku. When you
ran the ``heroku create`` command above, the ``heroku`` command added a new Git
remote to your project. To push your code to Heroku, all you do is push to the
``heroku`` remote::

    $ git push heroku master

That will 'deploy' your code straight to Heroku! From now on, whenever you want
to deploy your code, just run this command.


Step 2 - Install the Addons
---------------------------

Now that you've got your Heroku application going, let's install some `Heroku
Addons <https://addons.heroku.com/>`_. Heroku is a modular system. The core of
Heroku allows you to run your code, but doesn't provide any extra
infrastructure services.

To get things like PostgreSQL, memcache, RabbitMQ, etc.--you need to install
Heroku addons to do what you want.

Let's install our required addons now--these addons are all free (you can
upgrade them at any time in the future). ``django-skel`` already supports all
of these, and requires most of them to function::

    $ heroku addons:add cloudamqp:lemur
    $ heroku-postgresql:dev
    $ scheduler:standard
    $ memcache:5mb
    $ newrelic:standard
    $ pgbackups:auto-month
    $ sentry:developer

`cloudamqp <https://addons.heroku.com/cloudamqp>`_ is a hosted RabbitMQ
service. This is what makes our task queueing (via Celery) possible.

`heroku-postgresql <https://addons.heroku.com/heroku-postgresql>`_ is a hosted
PostgreSQL service that kicks ass.

`scheduler <https://addons.heroku.com/scheduler>`_ is a cron replacement.

`memcache <https://addons.heroku.com/memcache>`_ is a hosted memcache service.

`newrelic <https://addons.heroku.com/newrelic>`_ is the best application
monitoring tool ever created.

`pgbackups <https://addons.heroku.com/pgbackups>`_ is an excellent PostgreSQL
backup tool that stores backups automatically to S3, and lets you download and
manage your backups easily.

`sentry <https://addons.heroku.com/sentry>`_ is a pretty neat error aggregation
and searching tool that makes debugging issues simple.

Just for the record, if you'd like to upgrade any of these free addons, you can
do so by running the ``heroku addons:upgrade`` command. For example--to switch
from the free newrelic addon to their paid addon which has lots more features,
you can simply run::

    $ heroku addons:upgrade newrelic:professional

Bam!