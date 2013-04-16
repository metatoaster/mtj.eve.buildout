Introduction
============

This is the buildout for setting up the development environment for MTJ
EVE Online tools and application.

Requirements
------------

Python 2.7 is required.  Python 3 support may work, but I don't have it
installed so someone else can tell me whether it does or not.

Quick Start
-----------

The following examples assumes a POSIX like environment.

First run bootstrap::

    $ python bootstrap.py 
    Downloading http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11-py2.7.egg
    Creating directory '/home/buildout/mtj.eve.buildout/bin'.
    Creating directory '/home/buildout/mtj.eve.buildout/parts'.
    Creating directory '/home/buildout/mtj.eve.buildout/eggs'.
    Creating directory '/home/buildout/mtj.eve.buildout/develop-eggs'.
    Getting distribution for 'setuptools'.
    Got setuptools 0.6c12dev-r88846.
    Generated script '/home/buildout/mtj.eve.buildout/bin/buildout'.

Which will then generate the buildout script.  Run that like so::

    $ bin/buildout 
    mr.developer: Creating missing sources dir /home/buildout/mtj.eve.buildout/src.
    mr.developer: Queued 'mtj.eve.tracker' for checkout.
    mr.developer: Queued 'mtj.evedb' for checkout.
    mr.developer: Queued 'mtj.multimer' for checkout.
    mr.developer: Cloned 'mtj.evedb' with git.
    mr.developer: Cloned 'mtj.multimer' with git.
    mr.developer: Cloned 'mtj.eve.tracker' with git.
    Develop: '/home/buildout/mtj.eve.buildout/src/mtj.evedb'
    Develop: '/home/buildout/mtj.eve.buildout/src/mtj.eve.tracker'
    Develop: '/home/buildout/mtj.eve.buildout/src/mtj.multimer'
    Getting distribution for 'zc.recipe.egg'.
    Got zc.recipe.egg 1.3.2.
    ...
    Installing python.
    Getting distribution for 'evelink'.
    zip_safe flag not set; analyzing archive contents...
    Got EVELink 0.2.0.
    Getting distribution for 'sqlalchemy'.
    warning: no files found matching '*.jpg' under directory 'doc'
    no previously-included directories found matching 'doc/build/output'
    zip_safe flag not set; analyzing archive contents...
    Got SQLAlchemy 0.8.0b2.
    Generated interpreter '/home/buildout/mtj.eve.buildout/bin/python'.
    Installing test.
    Generated script '/home/buildout/mtj.eve.buildout/bin/test'.

Before any tests that depends on the eve datadumps (i.e. anything that
uses mtj.evedb) a suitable static data dump must be provided.  For
testing purposes, a limited subset of what CCP released is provided
within the mtj.evedb test module.  It is built on top of the sqlite data
dumps provided at http://pozniak.pl/dbdump/.  For production usage, just
download the latest version and place it somewhere on your drive and
make the appropriate initialization calls.  Please refer to the
documentations and tests provided by the mtj.evedb package.

Now run the evedb test like so::

    $ bin/test -s mtj.evedb
    Running zope.testrunner.layer.UnitTests tests:
      Set up zope.testrunner.layer.UnitTests in 0.000 seconds.
      Running:
                    
      Ran 6 tests with 0 failures and 0 errors in 0.310 seconds.
    Tearing down left over layers:
      Tear down zope.testrunner.layer.UnitTests in 0.000 seconds.

Alternatively just run ``bin/test`` without any arguments to run and see
the test results for all the buildout eggs.

The other interesting item is the local python interpreter.  It will
have all the right paths defined so you can toy with the modules using
the interactive shell::

    $ bin/python 

    >>> from mtj.evedb.map import Map
    >>> map = Map()
    >>> result = map.getSolarSystem(solarSystemName='Jita')
    >>> result['security']
    0.9459131166648388
    >>> from evelink.server import Server
    >>> server = Server()
    >>> server.server_status()
    {'players': 30942, 'online': True}

Lastly, a development flask application can be spawned like so::

    $ bin/python src/mtj.eve.tracker/mtj/eve/tracker/ctrl.py \
    >     -c tracker.config.json

Where the configuration file can be generated like this::

    $ bin/python src/mtj.eve.tracker/mtj/eve/tracker/ctrl.py
    mtj.tracker.ctrl> write_config tracker.config.json
    mtj.tracker.ctrl>

Edit the fields to what is required.
