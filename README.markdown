RIO
======
[![Build Status](https://travis-ci.org/albertosantini/node-rio.png)](https://travis-ci.org/albertosantini/node-rio)
[![NPM version](https://badge.fury.io/js/rio.png)](http://badge.fury.io/js/rio)
[![NGN Dependencies](https://david-dm.org/albertosantini/node-rio.png)](https://david-dm.org/albertosantini/node-rio)

RIO, R Input Output, connects an app to [Rserve](http://www.rforge.net/Rserve/),
a TCP/IP server which allows other programs to use facilities of [R](http://www.r-project.org).

It supports double, double array, string, string array boolean, boolean array
objects and raw vector (images or files).

It supports also the plain text authentication, if Rserve is configured for that
capability.

The main goal is to pass a string containing a script call using a JSON object
as parameter. Then, inside the script, using RJSONIO package, deserializing
the JSON object, calling a method, serializing the response and returning to
NodeJS.

Example
========

    var rio = require('rio');

    rio.evaluate("pi / 2 * 2");
    rio.evaluate('c(1, 2)');
    rio.evaluate("as.character('Hello World')");
    rio.evaluate('c("a", "b")');
    rio.evaluate('Sys.sleep(5); 11')

See `examples` directory.

- `ex1`: Getting started with `evaluate` api.
- `ex2`: How to evaluate a script, to call a function passing params with
`sourceAndEval`.
- `ex3`: How to call `sourceAndEval` with a config object including connection
details.
- `ex4`: An example with utf-8 chars.
- `ex5`: How to retrieve a plot.

Installation
============

[![NPM](https://nodei.co/npm/rio.png?downloads=true)](https://nodei.co/npm/rio/)
[![NPM](https://nodei.co/npm-dl/rio.png)](https://nodei.co/npm/rio/)

To install with [npm](http://github.com/isaacs/npm):

    npm install rio

Tested with node 0.10.x and Rserve 1.7.3 (on Windows 7) with R 3.1.0.

Don't forget to start [Rserve](http://cran.r-project.org/web/packages/Rserve/).
For instance, from R console, after installing the package Rserve:

    require('Rserve')
    Rserve()

To shutdown the server from R console:

    require('RSclient')
    c <- RSconnect()
    RSshutdown(c)

Methods
=======

evaluate(command, options)
--------------------------

Evaluate a command, connecting to Rserve, executing the command and then
disconnecting.

The defaults for the options parameter:

    options = {
        callback: function (err, res) {
            if (err) {
                util.puts(res);
            } else {
                util.puts("Rserve call failed. " + err);
            }
        },
        host = "127.0.0.1",
        port = 6311,
        path = undefined,
        user = "anon",
        password = "anon"
    }

Either define `path`, the path of a Unix socket, or `host`/`port`.


sourceAndEval(filename, options)
-------------

It loads the content of a R file and calls the `evaluate` method, merging,
finally, the options parameter:

    options = {
        entryPoint: "main", // entryPoint is called
        data: { foo: "bar" } // data is stringified and passed to entryPoint
    }

bufferAndEval(buffer, options)
-------------

It is an helper method to call `evaluate`, adding to the buffer string, the call
of the entry point. Options are the same as for `evaluate`.

shutdown(options)
-----------------

Sends the `CMD_shutdown` command to the Rserve server. Options are the same as
for `evaluate`.

enableDebug(isDebug)
-----------

It enables debugging mode, printing the packet and logging messages.

enableRecordMode(isRecordMode, options)
----------------

It enables record mode, dumping the incoming data to a file specified in the
options.

    options = {
        fileName: "node-rio-dump.bin"
    }

enablePlaybackMode(isPlaybackMode, options)
------------------

It enables playback mode, reading a dump file instead connecting to the server.

    options = {
        fileName: "node-rio-dump.bin"
    }

Contributors
============

```
 project  : node-rio
 repo age : 2 years, 9 months
 active   : 64 days
 commits  : 148
 files    : 38
 authors  :
   135  icebox                  91.2%
     6  albertosantini          4.1%
     4  Manuel Santillan        2.7%
     2  Anand Patil             1.4%
     1  Farrin Reid             0.7%
```
