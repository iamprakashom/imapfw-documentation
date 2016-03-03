# The rascal

> TODO: review, partially obsolete.



### The rascal

Here's a sample [rascal](https://github.com/OfflineIMAP/imapfw/tree/next/rascals):

``` python

__VERSION__ = 0.1


##########
# GLOBAL #
##########

#
# The main configuration options are set in this dict.
#
MainConf = {
    'max_sync_accounts': 2,
}


from imapfw.api import actions, engines, controllers, types, drivers
# Support for contributor's backends. ,-)
#from imapfw.api import contrib


################
# REPOSITORIES #
################

ImapExampleConf = {
    'dns':      'imap.gmail.com',
    'port':     '143',
    'username': 'myname',
    'max_connections': 2,
}

MaildirExampleConf = {
    'path': '~/Maildir',
    'max_connections': 2,
}

class MaildirRepositoryExample(types.Maildir):

    conf = MaildirExampleConf
    driver = drivers.Maildir # Default: drivers.Maildir.


class ImapRepositoryExample(types.Imap):

    conf = ImapExampleConf
    driver = ImapDriverExample # Default: drivers.Imap.

    def _getPassword(self):
        return self.conf.password

    def credentials(self):
        return self.conf.username, self._getPassword()

    def search(self, server, uid_list, conditions):
        title = mail.getTitle()

        if title.startswith('spam'):
            mail.deleteRemote('deleting spam')

        if title.startswith('offlineimap'):
            mail.setTitle("filtered: %s"% title)
            mail.moveTo('INBOX.offlineimap') # IMAP MOVE.

        return # Continue proceeding to the folder.

    def getFolders(self):
        return ['sample', 'of', 'static', 'remote', 'folder', 'list']



############
# ACCOUNTS #
############

class AccountExample(types.Account):
    left = MaildirRepositoryExample
    right = ImapRepositoryExample

    # Optional. The folders considered for a sync.
    # Arguments:
    # - folder_list: list of folder names to sync.
    # Returned values: the new list of folders to sync.
    def syncFolders(self, foldersList):
        return ['folderA', 'folderB', 'folderC']

```

* The first thing to notice is that configuration options are dictionnaries. Forget the INI style. I aim simple users to not have to write ONE line of Python code.
* OfflineIMAP users should feel comfortable reading this file. It follows almost the same semantic and the same logic around **accounts**.
* Most experienced users will notice the classes. Here is the hierarchy and how
  they are logically linked:
  * an account is defined (derivates from `types.Account`) with both `left` and `right` "repositories".
  * a "repository" *(I'm about to fully remove this term)* maps to a configuration dictionary and a driver to actually access it. It derivates from the associated type (`types.Maildir`, `types.Imap`, etc) and the drivers are similar (`drivers.Maildir`, `drivers.Imap`, etc).

On top of this, each **type** acts as a controller for the underlying driver. I expect experimented users to make most of their *"crappy"* things^W^W tuning here. ,-) Think about filtering folders, nametrans, etc.

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

