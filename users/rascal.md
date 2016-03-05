# The rascal

### The rascal

Here's a [sample rascal](https://github.com/OfflineIMAP/imapfw/blob/master/rascals/one.rascal.example) ([more rascals](https://github.com/OfflineIMAP/imapfw/tree/next/rascals)).

* The first thing to notice is that configuration options are dictionaries.
* OfflineIMAP users should feel comfortable reading this file. It follows almost the same semantic and the same logic around **accounts**.
* Most experienced users will notice the classes. Here is the hierarchy and how they are logically linked:
  * an account is defined (derives from `types.Account`) with both`left` and `right` "repositories".
  * a "repository" maps to a configuration dictionary and a driver to actually access it. It derives from the associated type (`types.Maildir`, `types.Imap`, etc) and the drivers are similar (`drivers.Maildir`, `drivers.Imap`, etc).

> TODO: review

How everything is linked is important: it's possible to decide what type uses which driver for what account.


Now, look at the imports:

``` python
from imapfw.api import actions, engines, types, drivers
```

* **actions** are the actions available from the command line.
* **engines** allows to define the default engine to use for the action (if this action supports more than one engine). It can be redefined at the CLI!
* **types** are the little sisters of the OfflineIMAP's repositories: Maildir, Imap, Gmail, etc, in the sense they hold a driver and some configuration options *(like the credentials)* to access it.
* **drivers** are the low-level drivers to use for the types (didn't exist in OfflineIMAP).

The imports are what makes the framework so powerfull. They allow users to **redefine** the default behaviour, or even write their own new full backends.

Because actions, engines, types and drivers are well orthogonal and distinct concepts, extending imapfw should be much easier than it was with OfflineIMAP.

