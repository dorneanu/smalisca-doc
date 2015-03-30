.. _page-analysis:

******************
Analyze results
******************

Basic usage
===========

After having parsed and collected valuable information about your 
application, you're ready to go with the analysis stuff.

.. note::

   At the moment only SQLite analysis is supported.

Your previously generated results should be located in a *SQLite* DB. 
But first let's have a look at the main options::

    $ smalisca analyzer --help

    ...

    usage: smalisca (sub-commands ...) [options ...] {arguments ...}

    [--] Analyze results using an interactive prompt or on the command line.

    optional arguments:
      -h, --help            show this help message and exit
      --debug               toggle debug output
      --quiet               suppress all output
      --log-level {debug,info,warn,error,critical}
                            Change logging level (Default: info)
      -i FILENAME, --input FILENAME
                            Specify results file to read from
      -f {sqlite}, --format {sqlite}
                            Files format
      -c COMMANDS_FILE      Read commands from file instead of interactive prompt

At the moment there are 2 ways how to interact with the results:

* using the provided **interactive prompt** (default)
* specifying a **file** containing **commands** to be executed (-c)

.. note::
 
   SQL ninjas could of course analyze the SQLite DB directly without any 3rd party
   applications. 

Interactive
-----------

In the **interactive** modus all you have to do is to specify the location of the 
SQLite DB and the format (which will be of course *sqlite*)::

    $ smalisca analyzer -i /tmp/fakebanker.sqlite -f sqlite

    ...

    :: INFO       Successfully opened SQLite DB
    :: INFO       Creating analyzer framework ...
    :: INFO       Starting new analysis shell

    -- Analyzer -------------------------------------------------------------------
    Welcome to smalisca analyzer shell. 
    Type ? or help to list available commands.
    Type "<command> --help" for additional help.

    smalisca>

Now you're ready to interact with the results. Just type **help** to see a list 
of available *commands*::

    smalisca>help

    Documented commands (type help <topic>):
    ----------------------------------------
    dc  dcl  dxcl  help  q  quit  sc  scl  sm  sp  sxcl

    smalisca>


For every provided type **<command> --help** to see a list of available options::

    smalisca>sc --help
    usage: sc [-h] [-c SEARCH_TYPE] [-p SEARCH_PATTERN] [-s SORTBY] [--reverse]
              [-r RANGE] [--max-width MAX_WIDTH] [-x EXCLUDE_FIELDS]

    [--] Search for classes

    Specify by '-c' in which column you'd like to search for a pattern (specified by '-p').
    Examples:

    a) List available columns
        sc -c ?

    b) Search for pattern "test" in column "class_name" (first 10 results)
        sc -c class_name -p test -r 10

    c) Search for pattern "test2" in column "class_type" (print only from index 10 to 20)
        sc -c class_type -p test2 -r 10,20

    You can also exclude table fields using '-x':

    a) Exclude only one column
        sc -c class_type -p test2 -x depth

    b) Exclude multiple columns:
        sc -c class_type -p test2 -x depth,id,class_name

    optional arguments:
      -h, --help            show this help message and exit
      -c SEARCH_TYPE        Specify column.
                            Type ? for list
      -p SEARCH_PATTERN     Specify search pattern
      -s SORTBY             Sort by column name
      --reverse             Reverse sort order
      -r RANGE              Specify output range by single integer or separated by ','
      --max-width MAX_WIDTH
                            Global column max width
      -x EXCLUDE_FIELDS     Exclude table fields
    smalisca>

In this specific case you could run::

    smalisca>sc -c ?
    ['id', 'class_name', 'class_type', 'class_package', 'depth', 'path']
    No results! :(
    smalisca>sc -c class_name -p gmail -x path -r 10
    +----+---------------------------------------------+--------------+-----------------------+-------+
    | id | class_name                                  | class_type   | class_package         | depth |
    +----+---------------------------------------------+--------------+-----------------------+-------+
    | 7  | Lcom/gmail/xservices/XService$MyRun         |              | Lcom.gmail.xservices  | 4     |
    | 13 | Lcom/gmail/xpack/R$id                       | public final | Lcom.gmail.xpack      | 4     |
    | 24 | Lcom/gmail/xlibs/myFunctions                | public       | Lcom.gmail.xlibs      | 4     |
    | 35 | Lcom/gmail/xpack/R$menu                     | public final | Lcom.gmail.xpack      | 4     |
    | 59 | Lcom/gmail/xservices/XSmsIncom$1RequestTask |              | Lcom.gmail.xservices  | 4     |
    | 68 | Lcom/gmail/xpack/R$raw                      | public final | Lcom.gmail.xpack      | 4     |
    | 69 | Lcom/gmail/xlibs/myFunctions$1RequestTask   |              | Lcom.gmail.xlibs      | 4     |
    | 81 | Lcom/gmail/xservices/XRepeat$1RequestTask   |              | Lcom.gmail.xservices  | 4     |
    | 88 | Lcom/gmail/xpack/R$style                    | public final | Lcom.gmail.xpack      | 4     |
    | 97 | Lcom/gmail/xbroadcast/OnBootReceiver        | public       | Lcom.gmail.xbroadcast | 4     |
    +----+---------------------------------------------+--------------+-----------------------+-------+


Batch like
----------
 
In the **batch** modues one could provide the commands in a file. These will be executed in that specific
order::

    $ cat cmd.txt
    sc -c class_name -p gmail -x path -r 10
    quit
    $ smalisca analyzer -i /tmp/fakebanker.sqlite -f sqlite -c cmd.txt
    
    ...

    :: INFO       Successfully opened SQLite DB
    :: INFO       Creating analyzer framework ...
    :: INFO       Reading commands from cmd.txt

    -- Analyzer -------------------------------------------------------------------
    Welcome to smalisca analyzer shell. 
    Type ? or help to list available commands.
    Type "<command> --help" for additional help.

    +----+---------------------------------------------+--------------+-----------------------+-------+
    | id | class_name                                  | class_type   | class_package         | depth |
    +----+---------------------------------------------+--------------+-----------------------+-------+
    | 7  | Lcom/gmail/xservices/XService$MyRun         |              | Lcom.gmail.xservices  | 4     |
    | 13 | Lcom/gmail/xpack/R$id                       | public final | Lcom.gmail.xpack      | 4     |
    | 24 | Lcom/gmail/xlibs/myFunctions                | public       | Lcom.gmail.xlibs      | 4     |
    | 35 | Lcom/gmail/xpack/R$menu                     | public final | Lcom.gmail.xpack      | 4     |
    | 59 | Lcom/gmail/xservices/XSmsIncom$1RequestTask |              | Lcom.gmail.xservices  | 4     |
    | 68 | Lcom/gmail/xpack/R$raw                      | public final | Lcom.gmail.xpack      | 4     |
    | 69 | Lcom/gmail/xlibs/myFunctions$1RequestTask   |              | Lcom.gmail.xlibs      | 4     |
    | 81 | Lcom/gmail/xservices/XRepeat$1RequestTask   |              | Lcom.gmail.xservices  | 4     |
    | 88 | Lcom/gmail/xpack/R$style                    | public final | Lcom.gmail.xpack      | 4     |
    | 97 | Lcom/gmail/xbroadcast/OnBootReceiver        | public       | Lcom.gmail.xbroadcast | 4     |
    +----+---------------------------------------------+--------------+-----------------------+-------+

Available commands
==================

s
--

[S]search for a **pattern** (-p) inside **all** tables. But you can also specify the **table** (-t) you'd like
to lookup your pattern::

    smalisca>s -p decrypt                                                                                    
    - Classes ---------------------------------------------------------------------
    :: WARNING    No found classes.

    - Properties ------------------------------------------------------------------
    :: WARNING    No found properties.

    - Const strings ---------------------------------------------------------------
    :: WARNING    No found const strings.

    - Methods ---------------------------------------------------------------------
    :: INFO       Found 1 results

    :: ID: 131

            [+] Name:       decrypt
            [+] Type:       public
            [+] Args:       Ljava/lang/String;
            [+] Ret:        Ljava/lang/String;
            [+] Class:      Lcom/gmail/xlibs/Blowfish


And now specifying the table::

    smalisca>s -t const -p container=
    - Classes ---------------------------------------------------------------------
    :: WARNING    No found classes.

    - Properties ------------------------------------------------------------------
    :: WARNING    No found properties.

    - Const strings ---------------------------------------------------------------
    :: INFO       Found 2 results

    :: ID: 39

            [+] Variable:   v0
            [+] Value:      mContainer=
            [+] Class:      Landroid/support/v4/app/Fragment



    :: ID: 833

            [+] Variable:   v6
            [+] Value:        mContainer=
            [+] Class:      Landroid/support/v4/app/FragmentManagerImpl


    - Methods ---------------------------------------------------------------------
    :: WARNING    No found methods.

sc
--

[S]earch for [c]lasses. You can search for a specific **pattern** (-p) in the available **columns**::

    smalisca>sc -c ?
    ['id', 'class_name', 'class_type', 'class_package', 'depth', 'path']

Example::

    smalisca>sc -c class_type -p public


sp
--

[S]earch for [p]roperties. You can search for a specific **pattern** (-p) in the available **columns**::

    smalisca>sp -c ?
    ['id', 'property_name', 'property_type', 'property_info', 'property_class']

Example::

    smalisca>sp -c property_class -p com/gmail


scs
---

[S]earch for [c]onstant [s]trings. You can search for a specific **pattern** (-p) in the available **columns**::

    smalisca>scs -c ?
    ['id', 'const_string_var', 'const_string_value', 'const_string_class']

Example::

    smalisca>scs -c const_string_value -p http
    +-----+------------------+--------------------+------------------------------+
    | id  | const_string_var | const_string_value | const_string_class           |
    +-----+------------------+--------------------+------------------------------+
    | 321 | v11              | HTTP               | Lcom/gmail/xlibs/myFunctions |
    | 337 | v10              | HTTP               | Lcom/gmail/xlibs/myFunctions |
    +-----+------------------+--------------------+------------------------------+


sm
--

[S]earch for [m]ethods. You can search for a specific **method** (-m) in the available **columns**::

    smalisca>sm -c ?
    ['id', 'method_name', 'method_type', 'method_args', 'method_ret', 'method_class']

Example::

    smlisca>sm -c method_ret -p I

scl
---

[S]earch for calls [cl]. Every call has a **source** (class, method) and a **destination** (class, method). 
Additionally a call can have several **parameters** and a **return** value. Using this command you can apply
several **filters** to each call "component"::

    smalisca>scl --help
    usage: scl [-h] [-fc FROM_CLASS] [-fm FROM_METHOD] [-tc TO_CLASS]
               [-tm TO_METHOD] [-fa LOCAL_ARGS] [-ta DEST_ARGS] [-s SORTBY]
               [--reverse] [-r RANGE] [--max-width MAX_WIDTH] [-x EXCLUDE_FIELDS]

    >> Search for calls

    You can apply filters by using the optional arguments.
    Without any arguments the whole 'calls' table will
    be printed.

    optional arguments:
      -h, --help            show this help message and exit
      -fc FROM_CLASS        Specify calling class (from)
      -fm FROM_METHOD       Specify calling method (from)
      -tc TO_CLASS          Specify destination class (to)
      -tm TO_METHOD         Specify destination method (to)
      -fa LOCAL_ARGS        Local arguments (from)
      -ta DEST_ARGS         Destination arguments (to)
      -s SORTBY             Sort by column name
      --reverse             Reverse sort order
      -r RANGE              smecify output range by single integer or separated by ','
      --max-width MAX_WIDTH
                            Global column max width
      -x EXCLUDE_FIELDS     Exclude table fields

Examples::

    smalisca>scl -fc com/gmail -fm init -r 10
    ...
    smalisca>scl -tm create 
    ...


sxcl
----

[S]earch for cross [x] calls [cl]. This command is very similar to the *scl* one and searches for calls
as well. *sxcl* allows you to search for calls that:

*  *refer* to a class and/or method or
* are *invoked from* a class and/or method

These are the main options::

    smalisca>sxcl --help
    usage: sxcl [-h] [-c CLASS_NAME] [-m METHOD_NAME] -d {to,from}
                [--max-depth [XREF_DEPTH]] [-s SORTBY] [--reverse] [-r RANGE]
                [--max-width MAX_WIDTH] [-x EXCLUDE_FIELDS]

    >> Search for calls

    You can apply filters by using the optional arguments.
    Without any arguments the whole 'calls' table will
    be printed.

    optional arguments:
      -h, --help            show this help message and exit
      -c CLASS_NAME         Specify class name
      -m METHOD_NAME        Specify method name
      -d {to,from}          Cros-reference direction
      --max-depth [XREF_DEPTH]
                            Cross-References max depth
                            Default: 1
      -s SORTBY             Sort by column name
      --reverse             Reverse sort order
      -r RANGE              smecify output range by single integer or separated by ','
      --max-width MAX_WIDTH
                            Global column max width
      -x EXCLUDE_FIELDS     Exclude table fields

You can specify a *class name* (-c) and/or a *method name* (-m). You can then define the *direction*
cross calls should be searched. To give you a better understand what this is about, let's say you have a 
method *myMethod* in class *MyClass*.

.. graphviz::

   digraph foo {
        rankdir=LR;
        subgraph cluster_0 {
            node [style=filled];
            method1 [label="myMethod()"];
            label = "MyClass";
            color=grey;
        }
   }


You may now want to find out classes/method which **point to** (-d to) to this class/method:

.. graphviz::

   digraph foo {
        rankdir=LR;
        
        subgraph cluster_1 {
            node [style=filled];
            o_1_method1 [label="myMethodX()"];
            o_1_method2 [label="myMethodY()"];
            label = "Other Class 1";
            color=grey;
        }
        
        subgraph cluster_2 {
            node [style=filled];
            o_2_method1 [label="myMethodX()"];
            o_2_method2 [label="myMethodY()"];
            label = "Other Class 2";
            color=grey;
        }

        subgraph cluster_0 {
            node [style=filled];
            method1 [label="myMethod()"];
            label = "MyClass";
            color=grey;
        }

        o_1_method2 -> method1;
        o_2_method1 -> method1;
   }

On the other side you may want to search for classes/methods which are **called/invoked** (-d from) by *MyClass*/*myMethod*:

.. graphviz::

   digraph foo {
        rankdir=LR;
        
        subgraph cluster_1 {
            node [style=filled];
            o_1_method1 [label="myMethodX()"];
            o_1_method2 [label="myMethodY()"];
            label = "Other Class 1";
            color=grey;
        }
        
        subgraph cluster_2 {
            node [style=filled];
            o_2_method1 [label="myMethodX()"];
            o_2_method2 [label="myMethodY()"];
            label = "Other Class 2";
            color=grey;
        }

        subgraph cluster_0 {
            node [style=filled];
            method1 [label="myMethod()"];
            label = "MyClass";
            color=grey;
        }

        method1 -> o_1_method2;
        method1 -> o_2_method1;
   }

Ok, what about classes/methods that are **invoked by the invoked** classes/methods? :)
Well for this purpose there is the **--max-depth** parameter which specifies to which 
depth the cross calls should be searchd. Let's have a look at some examples:

* *-d from --max-depth 0*

.. graphviz::

   digraph foo {
        rankdir=LR;
        subgraph cluster_0 {
            node [style=filled];
            method1 [label="myMethod()"];
            label = "MyClass";
            color=grey;
        }
   }

 
* *-d from --max-depth 1*

.. graphviz::

   digraph foo {
        rankdir=LR;
        
        subgraph cluster_1 {
            node [style=filled];
            o_1_method1 [label="myMethodX()"];
            o_1_method2 [label="myMethodY()"];
            label = "Other Class 1";
            color=grey;
        }
        
        subgraph cluster_2 {
            node [style=filled];
            o_2_method1 [label="myMethodX()"];
            o_2_method2 [label="myMethodY()"];
            label = "Other Class 2";
            color=grey;
        }

        subgraph cluster_0 {
            node [style=filled];
            method1 [label="myMethod()"];
            label = "MyClass";
            color=grey;
        }

        method1 -> o_1_method2;
        method1 -> o_2_method1;
   }

* *-d from --max-depth 2*

.. graphviz::

   digraph foo {
        rankdir=LR;
        
        subgraph cluster_1 {
            node [style=filled];
            o_1_method1 [label="myMethodX()"];
            o_1_method2 [label="myMethodY()"];
            label = "Other Class 1";
            color=grey;
        }
        
        subgraph cluster_2 {
            node [style=filled];
            o_2_method1 [label="myMethodX()"];
            o_2_method2 [label="myMethodY()"];
            label = "Other Class 2";
            color=grey;
        }

        subgraph cluster_3 {
            node [style=filled];
            o_3_method1 [label="myMethodX()"];
            o_3_method2 [label="myMethodY()"];
            label = "Other Class 3";
            color=grey;
        }

        subgraph cluster_4 {
            node [style=filled];
            o_4_method1 [label="myMethodX()"];
            o_4_method2 [label="myMethodY()"];
            label = "Other Class 4";
            color=grey;
        }

        subgraph cluster_0 {
            node [style=filled];
            method1 [label="myMethod()"];
            label = "MyClass";
            color=grey;
        }

        method1 -> o_1_method2;
        method1 -> o_2_method1;

        o_1_method2 -> o_3_method2;
        o_2_method1 -> o_4_method1;
   }

Got it? :)

Examples::

    smalisca>sxcl -c gmail -m init -d to --max-depth 1
    ...
    smalisca>sxcl -c gmail -m create -d from --max-depth 2
    ...
