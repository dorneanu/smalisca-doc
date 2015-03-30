.. _page-parsing:

****************
Parsing files
****************


As a very first step before conducting any analysis, you'll 
have to parse your Smali files for valueble information like:

* class names
* class properties
* class methods
* method calls

Once you have extracted this information from the files, you're
ready to go with the analysis. Using the **parser** sub-command 
you can parse a directory for Smali files::

    $ smalisca parser --help
    
    ...

    usage: smalisca (sub-commands ...) [options ...] {arguments ...}

    [--] Parse files and extract data based on Smali syntax.

    optional arguments:
      -h, --help            show this help message and exit
      --debug               toggle debug output
      --quiet               suppress all output
      --log-level {debug,info,warn,error,critical}
                            Change logging level (Default: info)
      -l LOCATION, --location LOCATION
                            Set location (required)
      -s SUFFIX, --suffix SUFFIX
                            Set file suffix (required)
      -f {json,sqlite}, --format {json,sqlite}
                            Files format
      -o OUTPUT, --output OUTPUT
                            Specify output file

The most important options for parsing are the *location* (-l) and the *suffix* (-s).
For exporting the results you'll have to specify the *output format* (-f) 
and the correponding *output file* (-o).

.. note::
    Make sure you **delete** the sqlite file before re-running the parser. Otherwise you
    might get confronted with DB errors.

Example::

    $ smalisca -l /tmp/APK-dumped -s java -f sqlite -o apk.sqlite
    ...


Create new parser
=================

You may also want to parse the files programmatically

.. highlight:: python
.. code-block:: python

    from smalisca.core.smalisca_main import SmaliscaApp
    from smalisca.modules.module_smali_parser import SmaliParser

    # Create new smalisca app
    # You'll have to create a new app in order to set logging
    # settins and so on. 
    app = SmaliscaApp()
    app.setup()

    # Set log level
    app.log.set_level('info')

    # Specify the location where your APK has been dumped
    location = '/tmp/APK-dumped'

    # Specify file name suffix
    suffix = 'java'

    # Create a new parser
    parser = SmaliParser(location, suffix)

    # Go for it!
    parser.run()

    # Get results
    results = parser.get_results()
