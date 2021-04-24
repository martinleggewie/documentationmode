

# The Documentation Mode - How to configure

If you have read the [How to use](how-to-use.md) part, you are now maybe under the impression that all this is not too easy and fun to apply.
Having a Trello board or a JIRA project in place might look more intriguing than fiddling around with fancy keyboard shortcuts in a text editor from the last millennium.
And yes, using graphical tools make more fun because they (normally) are more eye-candy and also easier to use.


## Motivation (or: why do we accept the bulkiness of Emacs and Git?)

To better understand why I have defined the rules and tools the way they are, let's have a look at the requirements I wanted/needed to meet.

The tooling must allow to

* start documenting an event like a meeting or a phone call quickly enough while the event is still ongoing,
* find information about an earlier event quickly while you are already taking part in a next event,
* get an overview of all the tasks which are currently ongoing and when they need to be completed,
* have several persons work independently from each other, but still be able to synchronize all changes as often as needed,
* work locally when there is no proper network connection for a limited period of time,
* run all the tooling on Windows, MacOS, and Linux,
* only use tools which are freely available.

If you can accept that these requirements are valid, then tools like Trello and JIRA have at least the "problem" that they always need a network connection.
And when working for - say - a bank, it can happen that you are on travel, need to document something, and have no such network connection at all.

_Note:
When I defined these requirements, I didn't know that soon the Covid19 pandemia would make remote work possible._


## How to set it all up

You have come a long way when you have reached this part of the document.
In this part I explain how to setup the tooling and also yourself to get everything up and running.


### How to setup yourself

Before you can start using the tools, you as a person maybe need to do some modifications to yourself :-)

* **You like to use keyboard shortcuts as the base mode of operation.**
This is the consequence of having a tooling in place which allows fast creation of documents:
You need to use the keyboard and a bunch of shortcuts instead of the mouse.
* **You are willing to learn a completely new set of keyboard shortcuts.**
Even if you already are a shortcut buff (like myself), you unfortunately need to learn a complete new set of shortcuts.
This can be confusing, especially if some of these shortcuts have a complete different meaning than what is used elsewhere.
Best examples:
    * The well-know "CTRL-c" will NOT copy selected text.
    Instead, this shortcut starts a sequence of subsequent shortcuts.
    * And the corresponding "CTRL-v" will NOT paste the current content of the clipboard to the cursor location.
    Instead, it will move the cursor one whole page down.

    As already said:
    Very confusing, I know.


### How to setup your computer

To get documentation mode rules up and running on your computer, you need to prepare the following things:


#### Install Git

As you will learn in a few moments, we use Git repos as the backbone of the communication between the team members.
Therefore you need to have a not too old version of Git installed – nothing special about this.
At the time of writing this text, I used Git version 2.27.0 on MacOS.


#### Configure Emacs and the included Org

You will be using Emacs and the included so-called "Org major mode" for most of the documentation.
Unfortunately, Emacs and especially Org mode are quite configuration-heavy.
Fortunately, Emacs is structured in such an open and extensible way that it is possible to have the needed configuration externalized in files and folders.
And externalizing the configuration is exactly what I have done.
All configuration is stored in three different Git repos.
Two repos are meant to be only used by yourself, whereas the third one is used to share information between all team members.

| Repo name       | Type         | Location in local file system | Description                                                                                                                                   |
|-----------------|--------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| TODO "emacs.d"  | Only for you | `~/.emacs.d`                  | Contains the basic Emacs configuration which mainly contains only a very short init.el file. Below you will find more information about this. |
| TODO "personal" | Only for you | `~/personal`                  | Contains that part of the created Org content which you consider to be only relevant for you. This is mainly the journal file.                |
| TODO "archedl"  | Shared       | `~/professional`              | This is the most important repo because it contains the Org Mode configuration and all the content relevant for all team members.             |

_TODO: Once I have setup the three demo Git repos, I will add the GitHub links here._

----

As a preparation for this document, I have written some overview to get this configuration structure explained in a slightly different way:

![Emacs configuration structure](emacs-configuration-structure.jpg)

----

As just mentioned in the table above, the init.el file is very short.
In fact, this file does nothing more than load three more configuration files:

* `~/.emacs.d/emacs-user-configuration.org`
* `~/.emacs.d/orgmode-common-configuration.org `
* `~/.emacs.d/orgmode-user-configuration.org`

As the `~/.emacs.d` Git repo does not contain these files itself, you need to provide them by yourself.

1.  emacs-user-configuration.org

    As the file name maybe implies, this file is meant to contain that part of the general Emacs configuration to follow your wishes.
    Main requirement is that you store the configuration in the "descriptive programming way" so that Emacs can load the files using the babel package.
    If you right now don't know what I am talking about, just contact me so that I can provide you my current configuration as a good starting point.

    Independently on how you define your Emacs configuration, I can only recommend storing this configuration file somewhere else in another Git repo.
    One typical pattern is to have another separate Git repo called "dotfiles" where you would store the real source of your Emacs configuration file.
    Independently on where you store the file with whatever name:
    In the end you only need to symlink your configuration file to `~/.emacs.d/emacs-user-configuration.org` so that Emacs can find and load it during starting phase.

2.  orgmode-common-configuration

    Here you don't have that much freedom on how this configuration should look like and where you store it.
    This `/.emacs.d/orgmode-common-configuration.org` file contains that part of the Org mode configuration which is relevant for the whole team.
    The true source of this file is supposed to be `~/professional/config/orgmode-configuration.org`, and you automatically got this file when you have cloned the "professional" Git repo.
    The only thing missing is that you also symlink this file to `~/.emacs.d/orgmode-common-configuration.org`.

3.  orgmode-user-configuration.org

    Not all parts of the Org mode configuration is suitable to be stored in the shared Git repo for the whole team.
    In order to get everything working, you most likely need to configure the following settings:

    * `user-full-name`: Contains your first and last name in one string, e.g. "Martin Leggewie".
    * `org-default-notes-file`: Contains the path to your refile file, e.g. "\~/professional/refile<sub>ml</sub>.org".
    * `org-default-journal-file`: Contains the path to your journal file, e.g. "\~/personal/journal.org".

    Again, it makes sense that you store the corresponding configuration file in another Git repo (e.g. "dotfiles").
    Then just symlink it to `~/.emacs.d/orgmode-user-configuration.org`.


#### Install Emacs

There is not much to say about this requirement.

* For Windows:
  * Navigate to <https://www.gnu.org/software/emacs/download.html>, and then scroll down to the "Windows" section.
  * Then navigate to the "nearby GNU mirror", and from there navigate to the latest Emacs version folder.
    By the time of writing, this has been "emacs-26".
  * From the list of all various Emacs bundles, download the one with the latest version including the dependencies.
    By the time of writing, this has been "emacs-26.3-x86<sub>64</sub>.zip".
  * Unzip this to a location of your liking.
  * Start Emacs by executing "bin/emacs.exe".

* For MacOS:
  * Navigate to <https://emacsformacosx.com>.
  * Download the latest version as a DMG file.
  * Mount the DMG file and copy the Emacs application folder to a place of your liking.
  * Maybe you have already guessed it:
    Start Emacs by executing the Emacs application.

If you have configured Emacs as described in the previous step, Emacs will try to download some packages.
Please allow it to do so.
It might be necessary that you configure a proxy server so that Emacs is able to connect to the Internet.
Put something like the following in your `emacs-user-configuration.org` file:

`(setq url-proxy-services '(("no_proxy" . "^\\(localhost\\|10.*\\)") ("http" . "localhost:8079") ("https" . "localhost:8079")))`

But in the end you should see the blank editor window.
(From my experience there will most likely be problems because Emacs is a complicated beast of software.
Sorry.)


----

_Martin Leggewie, 2021-04-24_

_Some meta information about the markdown files I have created for this document:_

* _I use the GitHub flavor of markdown._
_See [https://guides.github.com/features/mastering-markdown](https://guides.github.com/features/mastering-markdown) for how to use it._

* _I follow the one sentence per line approach which I came across several years ago when learning AsciiDoc(tor)._
_See [https://asciidoctor.org/docs/asciidoc-recommended-practices/#one-sentence-per-line](https://asciidoctor.org/docs/asciidoc-recommended-practices/#one-sentence-per-line) for the reasoning behind this approach._
