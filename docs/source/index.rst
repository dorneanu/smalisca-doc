.. smalisca documentation master file, created by
   sphinx-quickstart on Tue Mar  3 14:04:16 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Smalisca - Static Code analysis tool
====================================

If you ever have looked at Android applications you know to appreciate
the ability of analyzing your target at the most advanced level. Dynamic
programm analysis will give you a pretty good overview of your applications
activities and general behaviour. However sometimes you'll want to just 
analyze your application **without** running it. You'll want to have a look
at its components, analyze how they interact and how data is tainted 
from one point to another. 

This is was the major factor driving the development of *smalisca*. There
are indeed some good reasons for a *static code analysis* before the 
*dynamic* one. Before interacting with the application I like to know 
how the application has been build, if there is any API and generate all 
sort of *call flow graphs*. In fact graphs have been very important to 
me since they *visualize* things. Instead of jumping from file to file, 
from class to class, I just look at the graphs.

While graph building has been an important reason for me to code such a 
tool, *smalisca* has some other neat **features** you should read about.


Features
--------

At the moment there are some few major functionalities like:

* **parsing**

  You can parse a whole directory of **Smali** files and **extract**:
    
  * class information
  * class properties
  * class methods
  * calls between methods of different classes

  You can then **export** the results as **JSON** or **SQLite**. 

  Have a loot at the :ref:`parsing page <page-parsing>` for more information.



* **analyzing**

  After exporting the results you'll get an **interactive prompt** to take 
  a closer look at your parsed data. You can **search** for classes, properties, 
  methods and even method calls. You can then apply several **filters** to your search 
  criterias like::

      smalisca> sc -c class_name -p test -r 10 -x path -s class_type

  This command will search for *10* (-r 10) classes which contain the pattern *test* (-p)
  in their *class name* (-c). Afterwards  the command will exclude the column *path*
  (-x path) from the results and sort them by the *class type* (-s).

  Let's have a look at another example::
    
      smalisca> scl -fc com/android -fm init -r 10

  This will search for all **method calls** whose *calling* class name contains the pattern
  *com/android* (-fc). Additionally we can look for calls originating from methods whose
  name contain the pattern *init* (-fm). 

  You can of course read your commands from a file and analyze your results in a *batch*-
  like manner::

    $ cat cmd.txt
    sc -c class_name -p com/gmail/xlibs -r 10 -x path
    quit
    $ ./smalisca.py analyzer -i results.sqlite -f sqlite -c cmd.txt
    ...


  *Addition in version 0.2*: You can access the results via a web API.

  Have a loot at the :ref:`analysis page <page-analysis>` for more information.

* **web API**

  smalisca provides a REST web service in order to easily interact with the results by just using 
  a web client. This way you can access data in your own (fancy) web application and have a clean
  separation between backend and frontend. 

  Read more about the available REST API at the :ref:`web API page <page-web-api>`. 



* **visualizing**

  I think this the **most** valuable feature of *smalisca*. The ability to visualize your
  results in a structured way makes your life more comfortable. Depending on what you're 
  interested in, this tool has several graph drawing features I'd like to promote. 

  At first you can draw your packages including their classes, properties and methods::
    
    smalisca> dc -c class_name -p test -f dot -o /tmp/classes.dot
    :: INFO       Wrote results to /tmp/classes.dot
    smalisca> 
  
  This will first search classes whose class name contains *test* and then export the
  results in the **Graphviz DOT** language. You can then manually generate a graph using 
  *dot*, *neato*, *circo* etc. Or you do that using the interactive prompt::

    smalisca> dc -c class_name -p test -f pdf -o /tmp/classes.pdf --prog neato
    :: INFO       Wrote results to /tmp/classes.pdf
    smalisca>

  Have a loot at the :ref:`drawing page <page-drawing>` for more information.


Screenshots
===========

Have a look at the :ref:`screenshots page <page-screenshots>`.


Installation
============

Refer to the :ref:`installation page <page-installation>`.


How to use it
=============

After installing the tool, you may want to first pick up an Android application (APK) 
to play with. Use `apktool <https://code.google.com/p/android-apktool/>`_ or my own tool
`ADUS <https://github.com/dorneanu/adus>`_ to dump the APKs content. For the sake of 
simplicity I'll be using **FakeBanker** which I've analyzed in a previous 
`blog post <http://blog.dornea.nu/2014/07/07/disect-android-apks-like-a-pro-static-code-analysis/>`_. 

First touch
-----------

But first let's have a look at the tools main options::

    $ smalisca --help

                               /\_ \    __                            
      ____    ___ ___      __  \//\ \  /\_\    ____    ___     __     
     /',__\ /' __` __`\  /'__`\  \ \ \ \/\ \  /',__\  /'___\ /'__`\   
    /\__, `\/\ \/\ \/\ \/\ \L\.\_ \_\ \_\ \ \/\__, `\/\ \__//\ \L\.\_ 
    \/\____/\ \_\ \_\ \_\ \__/.\_\/\____\\ \_\/\____/\ \____\ \__/.\_\
     \/___/  \/_/\/_/\/_/\/__/\/_/\/____/ \/_/\/___/  \/____/\/__/\/_/
                                                                      

    --------------------------------------------------------------------------------
    :: Author:       Victor <Cyneox> Dorneanu
    :: Desc:         Static Code Analysis tool for Smali files
    :: URL:          http://nullsecurity.net, http://{blog,www}.dornea.nu
    :: Version:      0.2
    --------------------------------------------------------------------------------

    usage: smalisca (sub-commands ...) [options ...] {arguments ...}

    [--] Static Code Analysis (SCA) tool for Baskmali (Smali) files.

    commands:

      analyzer
        [--] Analyze results using an interactive prompt or on the command line.

      parser
        [--] Parse files and extract data based on Smali syntax.

      web
        [--] Analyze results using web API.

    optional arguments:
      -h, --help            show this help message and exit
      --debug               toggle debug output
      --quiet               suppress all output
      --log-level {debug,info,warn,error,critical}
                            Change logging level (Default: info)
      -v, --version         show program's version number and exit


Parsing
-------

I'll first **parse** some directory for **Smali** files before doing the analysis stuff:: 
    
    $ smalisca parser -l ~/tmp/FakeBanker2/dumped/smali -s java -f sqlite  -o fakebanker.sqlite

    ...

    :: INFO       Parsing .java files in /home/victor/tmp/FakeBanker2/dumped/smali ... 
    :: INFO       Finished parsing!
    :: INFO       Exporting results to SQLite
    :: INFO         Extract classes ...
    :: INFO         Extract class properties ...
    :: INFO         Extract class methods ...
    :: INFO         Extract calls ...
    :: INFO         Commit changes to SQLite DB
    :: INFO         Wrote results to fakebanker.sqlite
    :: INFO       Finished scanning

Also have a look at the :ref:`parsing page <page-parsing>` for further information.


Analyzing
----------

Now you're free to do whatever you want with your generated exports. You can inspect the **SQLite DB**
directly or use *smaliscas* **analysis** features::
    
    $ smalisca analyzer -f sqlite -i fakebanker.sqlite

    ...


    smalisca>sc -x path -r 10
    +----+-----------------------------------------------------------------------------------------+--------------------+--------------------------+-------+
    | id | class_name                                                                              | class_type         | class_package            | depth |
    +----+-----------------------------------------------------------------------------------------+--------------------+--------------------------+-------+
    | 1  | Landroid/support/v4/net/ConnectivityManagerCompat                                       | public             | Landroid.support.v4.net  | 5     |
    | 2  | Landroid/support/v4/view/AccessibilityDelegateCompat$AccessibilityDelegateJellyBeanImpl |                    | Landroid.support.v4.view | 5     |
    | 3  | Landroid/support/v4/view/ViewCompat$ViewCompatImpl                                      | interface abstract | Landroid.support.v4.view | 5     |
    | 4  | Landroid/support/v4/app/ActivityCompatHoneycomb                                         |                    | Landroid.support.v4.app  | 5     |
    | 5  | Landroid/support/v4/app/NoSaveStateFrameLayout                                          |                    | Landroid.support.v4.app  | 5     |
    | 6  | Landroid/support/v4/net/ConnectivityManagerCompatHoneycombMR2                           |                    | Landroid.support.v4.net  | 5     |
    | 7  | Lcom/gmail/xpack/BuildConfig                                                            | public final       | Lcom.gmail.xpack         | 4     |
    | 8  | Landroid/support/v4/app/BackStackRecord$Op                                              | final              | Landroid.support.v4.app  | 5     |
    | 9  | Landroid/support/v4/app/FragmentManagerImpl                                             | final              | Landroid.support.v4.app  | 5     |
    | 10 | Landroid/support/v4/app/ShareCompat$ShareCompatImpl                                     | interface abstract | Landroid.support.v4.app  | 5     |
    +----+-----------------------------------------------------------------------------------------+--------------------+--------------------------+-------+
  
Also refer to the :ref:`analysis page <page-analysis>` for more available **commands** and options.


Drawing
-------

Please refer to the :ref:`drawing page <page-drawing>` for full examples.

 

Contents
--------

.. toctree::
   :maxdepth: 3

   installation
   parsing
   analysis
   web-api
   drawing
   screenshots 

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

