.. _page-drawing:

****************
Draw results
****************

Basic usage
============

All **drawing** commands start with a **d**. The d-commands will first invoke a 
s-command (search) to get the results and generate a graph afterwards.  You can 
specify different output formats (by -f) which best fit to your needs. A required
output file name (-o) also has to be specified. 

.. note::

    Don't miss the :ref:`screenshots <page-screenshots>`.


Available commands
==================

dc
--

[D]raws [c]lass graphs. Similar to the *sc* command you can search for classes by specifying the column type (-c) 
and a pattern (-p). General usage::

    smalisca>dc --help
    usage: dc [-h] [-c SEARCH_TYPE] [-p SEARCH_PATTERN]
              [-f {dot,xdot,png,pdf,jpg,svg}]
              [--prog {dot,neato,circo,twopi,fdp,sfdp,nop}] [--args OUTPUT_ARGS]
              -o OUTPUT

    >> Draw class graphs

    optional arguments:
      -h, --help            show this help message and exit
      -c SEARCH_TYPE        Specify column.
                            Type ? for list
      -p SEARCH_PATTERN     Specify search pattern
      -f {dot,xdot,png,pdf,jpg,svg}
                            Output format
                            Default: dot
      --prog {dot,neato,circo,twopi,fdp,sfdp,nop}
                            Graphviz layout method
                            Default: dot
      --args OUTPUT_ARGS    Additional graphviz arguments
      -o OUTPUT             Specify output file

Examples::

    smalisca>dc -c class_name -p com/gmail/xservices -f dot -o /tmp/calls.dot
    :: INFO       Wrote results to /tmp/calls.dot

You can of course use other **output formats**::

    smalisca>dc -c class_name -p com/gmail/xservices -f png -o /tmp/calls.png
    :: INFO       Wrote results to /tmp/calls.png

Or a different **graphviz engine**::

    smalisca>dc -c class_name -p com/gmail/xservices -f png --prog fdp -o /tmp/calls.png
    :: INFO       Wrote results to /tmp/calls.png

dcl
---

[D]raws calls [cl].  Similar to the *scl* command you can search for calls by specifiny the calling method/class 
and/or the destinated method/class. General usage::

    smalisca>dcl --help
    usage: dcl [-h] [-fc FROM_CLASS] [-fm FROM_METHOD] [-tc TO_CLASS]
               [-tm TO_METHOD] [-fa LOCAL_ARGS] [-ta DEST_ARGS]
               [-f {dot,xdot,png,pdf,jpg,svg}]
               [--prog {dot,neato,circo,twopi,fdp,sfdp,nop}] [--args OUTPUT_ARGS]
               -o OUTPUT

    >> Draw calls graphs

    optional arguments:
      -h, --help            show this help message and exit
      -fc FROM_CLASS        Specify calling class (from)
      -fm FROM_METHOD       Specify calling method (from)
      -tc TO_CLASS          Specify destination class (to)
      -tm TO_METHOD         Specify destination method (to)
      -fa LOCAL_ARGS        Local arguments (from)
      -ta DEST_ARGS         Destination arguments (to)
      -f {dot,xdot,png,pdf,jpg,svg}
                            Output format
                            Default: dot
      --prog {dot,neato,circo,twopi,fdp,sfdp,nop}
                            Graphviz layout method
                            Default: dot
      --args OUTPUT_ARGS    Additional graphviz arguments
      -o OUTPUT             Specify output file

Let's have a look at some examples::

    smalisca>dcl -fc gmail -tm create -f pdf --prog fdp -o /tmp/smalisca/calls.pdf
    :: INFO       Wrote results to /tmp/smalisca/calls.pdf


dxcl
----

[D]raws cross [x] calls [cl]. Similar to the *sxcl* command you can search for cross-calls by specifying 
class name and/or method name. General usage:: 

    smalisca>dxcl --help
    usage: dcxl [-h] [-c CLASS_NAME] [-m METHOD_NAME] -d {to,from}
                [--max-depth [XREF_DEPTH]] [-f {dot,xdot,png,pdf,jpg,svg}]
                [--prog {dot,neato,circo,twopi,fdp,sfdp,nop}] [--args OUTPUT_ARGS]
                -o OUTPUT

    >> Draw cross-calls graphs

    optional arguments:
      -h, --help            show this help message and exit
      -c CLASS_NAME         Specify class name
      -m METHOD_NAME        Specify method name
      -d {to,from}          Cros-reference direction
      --max-depth [XREF_DEPTH]
                            Cross-References max depth
                            Default: 1
      -f {dot,xdot,png,pdf,jpg,svg}
                            Output format
                            Default: dot
      --prog {dot,neato,circo,twopi,fdp,sfdp,nop}
                            Graphviz layout method
                            Default: dot
      --args OUTPUT_ARGS    Additional graphviz arguments
      -o OUTPUT             Specify output file

Let's have a look at some examples::

    smalisca>dxcl -c gmail/xlibs -d to --max-depth 1 -f pdf --prog dot -o /tmp/smalisca/xcalls.pdf
    :: INFO       Namespace(to_class='gmail/xlibs')
    :: INFO       Wrote results to /tmp/smalisca/xcalls.pdf
 



