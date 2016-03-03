# Getting started

A good start is to run the command and look at the CLI options:

``` bash
$ ./imapfw.py -h
usage: imapfw [-h] [--log-level {0,1,2,3}] [-c {multiprocessing,threading}]
              [-r RASCAL]
              [-d {all,callbacks,imap,controllers,drivers,architects,managers,emitters,workers}]
              [-v]
              ACTION ...

Copyright 2016 Nicolas Sebrecht & contributors. The MIT License (MIT).

optional arguments:
  -h, --help            show this help message and exit
  --log-level {0,1,2,3}
                        define the logging level for the output (default is 3)
  -c {multiprocessing,threading}
                        the concurrency backend to use (default is
                        multiprocessing)
  -r RASCAL             the rascal file to use
  -d {all,callbacks,imap,controllers,drivers,architects,managers,emitters,workers}, --debug {all,callbacks,imap,controllers,drivers,architects,managers,emitters,workers}
                        enable debugging for the requested partial(s)
  -v                    show program's version number and exit

Available actions:
  Each action might allow its own options. Run 'imapfw ACTION -h' to know
  more.

  ACTION
    unitTests           run the integrated unit tests
    noop                test if the rascal can be loaded
    testRascal          test your rascal
    examine             examine repositories
    shell               run in shell mode
    devel               development
    syncAccounts        sync on or more accounts
```

The main argument is the `ACTION`. It tells what imapfw must do. I've started the implementation of the most expected feature: `syncAccounts`. For now, it won't sync anything and you can run it safely.

