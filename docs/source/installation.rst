.. _page-installation:

*************
Installation
*************

The installation process is quite straight forward. Download tool from `GitHub <https://github.com/dorneanu/smalisca/>`_
or clone the whole project locally::

    $ git clone https://github.com/dorneanu/smalisca
    ...

Virtual environment
===================

You may now want to setup a virtual local environment::

    $ mkdir env
    $ virtualenv env
    ...
    $ source env/bin/activate

Install package
===============

Now into the packages root directory and install the package:
  
    $ cd smalisca
    $ make install 
    ...

Using PyPI
----------
smalisca is also available at PyPI. You may want to install it using::

    $ pip install smalisca
    

That's it! Now you're ready to run *smalisca*.


Uninstall package
=================

From the root directory run::

    $ cd smalisca
    $ make uninstall

