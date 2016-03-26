
Let's start with the global architecture.

## How the program boots

> A top-down approach.

The Python file called from the command line is [`imapfw.py`]({{ sources }}/imapfw.py). Look at the imports:

``` python
from imapfw import Imapfw
```

It first imports the [`imapfw` package]({{ sources }}/imapfw). As a good Python programer, you know that when importing a module of a package, Python first executes the `__init__.py` file(s) of this package (from high-level to down-level).

So here, it loads and executes [`imapfw/__init__.py`]({{ sources }}/imapfw/__init__.py). In this file, we have:

``` python
from imapfw.init import Imapfw
```

Since `init` is in the same package, we can check this [`imapfw/init.py`]({{ sources }}/imapfw/init.py) out. Bingo! We have the starting code. Reading this file, you should figure that its job is to:

* initialize the environment;
* load and run the action requested at the CLI.


## The components of the imapfw package

The modules and sub-packages in [`imapfw`]({{ sources }}/imapfw):

* **actions/** \\
Package of all the available actions at CLI. Each action is only using the required ressources it needs from what imapfw provides.

* **annotation.py** \\
The Python3 annotations used in the codebase.

* **api/** \\
The exposed API. The purpose is to make proper indirection of the real code to allow higher flexibility and a better organization. Used in the `rascal`.

* **architects/** \\
Internal library to factorize code dedicated to the art of architecturing. This provides predefined environements/architectures handling workers, communication between workers, etc.

* **concurrency/** \\
Package providing concurrency.

* **conf/** \\
Package about configuring the runtime environement. Also, enables access to the CLI options.

* **constants.py** \\
Where to store the constants.

* **controllers/** \\
Controllers available, to be used on top of a driver.

* **drivers/** \\
Low-level to manipulate data of a repository (Maildir, IMAP, etc).

* **edmp.py** \\
Event-driven message passing: communication between workers.

* **engines/** \\
Engines allow to apply some processing logic to the connected end-point(s) (e.g.: synchronization).

* **error.py** \\
Exceptions used in imapfw.

* **imap/** \\
Handles the IMAP protocol.

* **interface.py** \\
Allow to check the internal/public interfaces. Reinforce unit testing and legacy compatibility.

* **offlineimap/** \\
Internal package dedicated to the `offlineimap` action.

* **rascal.py** \\
Package to access the rascal. The rascal is the "configuration file" defined by the user. The framework will use objects and call functions defined here.

* **runners/** \\
A runner is the function to call in a worker. Simple runners *(not likelly to change)* are put here.
* **runtime.py** \\
Module to handle global variables.

* **shells/** \\
Package dedicated to the `shell` action.

* **testing/** \\
Package dedicated to the `unittests` action.

* **toolkit.py** \\
Some factorized features that are not worth a dedicated module.

* **types/** \\
The high-level types imapfw is working with (messages, repositories, accounts, etc). Expect them to be used by advanced users from the rascal.

* **ui/** \\
Package for the user interface.
