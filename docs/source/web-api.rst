.. _page-web-api:

******************
Web API
******************

In order to improve usability one could use a web API to access 
previously generated results. At the moment a *REST* like API 
is available to access:

* classes
* class methods
* class properties
* constant strings
* calls
* cross-calls

Actually you should be able to access every *table* inside your DB 
using a web client.

Basic usage
===========

Before being able to access any data make sure you have parsed 
your Smali files properly and exported the results to a **SQLite**
(*-f sqlite*) DB. Afterwards you can start your web service::

    $ smalisca web --help

    ...
    Usage: smalisca (sub-commands ...) [options ...] {arguments ...}

    [--] Analyze results using web API.

    optional arguments:
      -h, --help            show this help message and exit
      --debug               toggle debug output
      --quiet               suppress all output
      --log-level {debug,info,warn,error,critical}
                            Change logging level (Default: info)
      -f FILENAME, --file FILENAME
                            Specify SQLite DB (required)
      -H HOST, --host HOST  Specify hostname to listen on
      -p PORT, --port PORT  Specify port to listen on


As an example you start the web server this way::

    $ smalisca web -H localhost -p 1337 -f /tmp/fakebanker.sqlite

                              /\_ \    __                            
      ____    ___ ___      __  \//\ \  /\_\    ____    ___     __     
     /',__\ /' __` __`\  /'__`\  \ \ \ \/\ \  /',__\  /'___\ /'__`\   
    /\__, `\/\ \/\ \/\ \/\ \L\.\_ \_\ \_\ \ \/\__, `\/\ \__//\ \L\.\_ 
    \/\____/\ \_\ \_\ \_\ \__/.\_\/\____\\ \_\/\____/\ \____\ \__/.\_\
     \/___/  \/_/\/_/\/_/\/__/\/_/\/____/ \/_/\/___/  \/____/\/__/\/_/
                                                                      
                                                                      
    ...


    :: INFO       Successfully opened SQLite DB
    :: INFO       Starting web application ...
     * Running on http://localhost:1337/ (Press CTRL+C to quit)

Now you can fetch data from the web application using a **HTTP client**
or a web browser. 


REST API
========

The REST API of smalisca is based on `Flask <http://flask.pocoo.org/>`_ and `Flask-Restless <https://flask-restless.readthedocs.org/en/latest/>`_. As stated in the documenation::

    Flask-Restless provides simple generation of ReSTful APIs for database 
    models defined using SQLAlchemy (or Flask-SQLAlchemy). The generated APIs 
    send and receive messages in JSON format.

Dito. So let's have a look at the basic functionalities:

* retrieve *data* via GET requests::

  GET /api/<table name>

* depending on the SQL table you'd like to receive entries from, you can specify the ID of a certain entry::

  GET /api/<table name>/(int: id)

*  you can apply **filters** to the queried results::

  GET /api/<table name>?q=<filters>


.. note::
   At the moment only following HTTP methods are allowed:
        * GET
        * POST

Now let's have a look at some examples::

a) Retrieve *some* classes::

    $ curl http://localhost:1337/api/classes

    {
      "num_results": 335,
      "objects": [
        {
          "class_name": "Landroid/support/v4/app/BackStackRecord$Op",
          "class_package": "Landroid.support.v4.app",
          "class_type": "final",
          "const_strings": [],
          "depth": 5,
          "id": 1,
          "methods": [
            {
              "id": 1,
              "method_args": "",
              "method_class": "Landroid/support/v4/app/BackStackRecord$Op",
              "method_name": "<init>",
              "method_ret": "V",
              "method_type": "constructor"
            }
          ],
          "path": "/home/victor/tmp/FakeBanker2/dumped/smali/android/support/v4/app/BackStackRecord$Op.java",
          "properties": [
            {
              "id": 1,
              "property_class": "Landroid/support/v4/app/BackStackRecord$Op",
              "property_info": "",
              "property_name": "fragment",
              "property_type": "Landroid/support/v4/app/Fragment"
            },
            {
              "id": 2,
              "property_class": "Landroid/support/v4/app/BackStackRecord$Op",
              "property_info": "",
              "property_name": "next",
              "property_type": "Landroid/support/v4/app/BackStackRecord$Op"
            },
            {
              "id": 3,
              "property_class": "Landroid/support/v4/app/BackStackRecord$Op",
              "property_info": "",
              "property_name": "prev",
              "property_type": "Landroid/support/v4/app/BackStackRecord$Op"
            },
            {
              "id": 4,
              "property_class": "Landroid/support/v4/app/BackStackRecord$Op",
              "property_info": "",
              "property_name": "removed",
              "property_type": "Ljava/util/ArrayList"
            }

          ...
      "page": 1,
      "total_pages": 34
      }

    
b) Get *class* entry (id = **2**)::

    $ curl http://localhost:1337/api/classes/2

    {
      "class_name": "Landroid/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1",
      "class_package": "Landroid.support.v4.view.accessibility",
      "class_type": "",
      "const_strings": [],
      "depth": 6,
      "id": 2,
      "methods": [
        {
          "id": 2,
          "method_args": "",
          "method_class": "Landroid/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1",
          "method_name": "<init>",
          "method_ret": "V",
          "method_type": "constructor"
        },
        {
          "id": 3,
          "method_args": "Ljava/lang/Object;",
          "method_class": "Landroid/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1",
          "method_name": "getMaxScrollX",
          "method_ret": "I",
          "method_type": "public static"
        },
        {
          "id": 4,
          "method_args": "Ljava/lang/Object;",
          "method_class": "Landroid/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1",
          "method_name": "getMaxScrollY",
          "method_ret": "I",
          "method_type": "public static"
        },
        {
          "id": 5,
          "method_args": "Ljava/lang/Object;I",
          "method_class": "Landroid/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1",
          "method_name": "setMaxScrollX",
          "method_ret": "V",
          "method_type": "public static"
        },
        {
          "id": 6,
          "method_args": "Ljava/lang/Object;I",
          "method_class": "Landroid/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1",
          "method_name": "setMaxScrollY",
          "method_ret": "V",
          "method_type": "public static"
        }
      ],
      "path": "/home/victor/tmp/FakeBanker2/dumped/smali/android/support/v4/view/accessibility/AccessibilityRecordCompatIcsMr1.java",
      "properties": []
    }% 

c) Get **4**-th page of the results::

    $ curl http://localhost:1337/api/classes?p=4

d) Apply filters to query results

    * Get all classes where class_name LIKE %android%::

        $ curl -v -G -H "Content-Type: application/json" \ 
                -d 'q={"filters":[{"name":"class_name","op":"like","val":"%Creator%"}]}' \
                http://localhost:1337/api/classes

        {
          "num_results": 4,
          "objects": [
            {
              "class_name": "Landroid/support/v4/os/ParcelableCompatCreatorCallbacks",
              "class_package": "Landroid.support.v4.os",
              "class_type": "public interface abstract",
              "const_strings": [],
              "depth": 5,
              "id": 10,
              "methods": [
                {
                  "id": 89,
                  "method_args": "Landroid/os/Parcel;Ljava/lang/ClassLoader;",
                  "method_class": "Landroid/support/v4/os/ParcelableCompatCreatorCallbacks",
                  "method_name": "createFromParcel",
                  "method_ret": "Ljava/lang/Object;",
                  "method_type": "public abstract"
                },
                {
                  "id": 90,
                  "method_args": "I",
                  "method_class": "Landroid/support/v4/os/ParcelableCompatCreatorCallbacks",
                  "method_name": "newArray",
                  "method_ret": "[Ljava/lang/Object;",
                  "method_type": "public abstract"
                }
              ],
              "path": "/home/victor/tmp/FakeBanker2/dumped/smali/android/support/v4/os/ParcelableCompatCreatorCallbacks.java",
              "properties": []
            },

           ...
        }

    * Get all classes where id > 10::

        $ curl -v -G -H "Content-Type: application/json" \
               -d 'q={"filters":[{"name":"id","op":"ge","val":10}]}' \
                http://localhost:1337/api/classes

        ...

    * Get all classes where id > 10 AND class_type like "%final%"::

       $ curl -v -G -H "Content-Type: application/json" \
              -d 'q={"filters":[{"and":[{"name":"id","op":"ge","val":10},{"name":"class_type","op":"like","val":"%final%"}]}]}' \
              http://localhost:1337/api/classes
       
       ...

.. note::
   For additional examples make sure you have a look at `Making search queries <https://flask-restless.readthedocs.org/en/latest/searchformat.html#making-search-queries>`_ inside the Flask-Restless documentation.






                             




