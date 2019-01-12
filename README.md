# the-adventure-game
A placeholder for the Adventure Game software from a defunct website by Phantom-Inker. The original ReadMe goes as follows as taken from The Wayback Machine.

## AG: The Adventure Game

## OVERVIEW

AG is a simple adventure game that allows visitors to your
web site to add episodes in a growing story.  It is like a
"Choose Your Own Adventure" (TM) book, only at the end of
each page, if the option does not go to a page that exists,
the reader is invited to create it.

AG conceptually owes a lot to the original Addventure, but
does not share the same code or the same design (notably,
Addventure is written in Perl, and AG is written in PHP).
AG includes administrative functions, as well, so that
content can be controlled somewhat.  It also includes a
Smarty templating engine, so that it can integrate better
into your web site; BBCode support, so that authors can
format their episodes however they want, and a spell-
checker to prevent errors.  AG uses flat files for all
storage, so it doesn't require a database.

## INCLUDED PACKAGES AND REQUIREMENTS

AG is written in PHP and requires PHP version 4.1 or higher.
Spell-checking support requires PHP to have the "pspell"
package to be compiled in (most Un*x/Linux systems have this).

AG has been tested only on Linux servers running Apache.
Other servers may require some massaging to get AG to work.

AG uses several off-the-shelf packages:

    Smarty Template Engine - version 2.6.0
    http://smarty.php.net

    Advanced BBCode - version 1.2 - Yves Goergen
    http://software.unclassified.de/abbc
    Note:  This has been modified slightly for AG, but
        an upgraded version would work without many
        changes.

    Spell Checker - version 2.9 - by Garrison Locke
    http://www.broken-notebook.com/spell_checker
    Note:  The spell-checker code used by AG is derived
        from this spell-checker, but has some significant
        differences.  Do *not* attempt to replace AG's
        spell-checker with an upgraded version from the
        above web site; you *will* break AG if you do.


## PACKAGE CONTENTS

You should receive AG as a tarball named something like
"ag-1.3.tar.gz".  Inside this tarball, you should see
the following files:

    ag/
        README           This file
        ag_core.php      Shared by all parts of AG

        admin_*.php      Various administration pages
        admin.php        The core administration system

        cancel.php       Used to cancel creating/editing
        common.js        Misc. creating/editing Javascript
        create.php       Lets user write a new episode
        index.php        Redirector page
        preview.php      Displays previews when creating
        read.php         Formats and displays episodes
        submit.php       Creates new episode files
        search.php       The AG search engine

        abbc/            Part of the BBCode processor
        abbc.*.php       Part of the BBCode processor
        smilies/         "Smiley" graphic images

        smarty/          Smarty template processor
        config/          Smarty config files
        templates/       Smarty templates
          default/       Default template set
            create.tpl   Episode-creation template
            css.tpl      Shared CSS
            dne.tpl      Episode does not exist error
            edit.tpl     Admin-episode-edit template
            error.tpl    For generic error messages
            main.tpl     Main template used by all pages
            preview.tpl  Formats episodes for preview
            read.tpl     Formats episodes for reading
            start.tpl    Formats episode #1 for reading

        old/             Contains old or rejected code

        config.php       Global configuration file
        data/            Data directory


## NEW INSTALLATION INSTRUCTIONS

Follow these instructions only if you have never installed
AG before.  If you have an existing installation you wish
to upgrade, follow the upgrade instructions in the next
section.

### Step 1.  Unpack the tarball.

You should have received AG as a tarball titled something
like ag-1.3.tar.gz.  Unpack the contents of this tarball
to yield a directory named "ag" with various files under it.
AG only refers to its code and data using relative paths,
so you can rename this directory to the name of your game,
if you like.

    Example commands:
    `> tar -xzvf ag-1.3.tar.gz`

### Step 2.  Set Permissions.

Go into AG's directory and set the data/ directory to be
writable by your web server.  Go into the data/ directory
and set all of the files and directories you see to be
writable by your web server.  Go into the 000/ directory
and set all of its files to be writable by your web server.
Also set both Smarty cache directories to be writable by
your web server.

    Example commands:
   `> cd ag/`
   `> chmod 777 config
   `> chmod 777 smarty/templates_c
   `> chmod 777 data
   `> cd data/
   `> chmod 666 *
   `> chmod 777 000
   `> cd 000/
   `> chmod 666 *

Note that the tarball *should* correctly set these
permissions when it's created, but if you're having
trouble installing AG, this is the first place to check.

### Step 3.  Setup an administrator (or two).

Go into AG's directory and open the "config.php" file in
your favorite text editor.  Change the name of the game
to whatever your game should be.  You shouldn't have to
change the timeout.  Then add an administrator (or two).
One administrator, "admin", is added by default, with a
password of "password".  You should change the password
at a minimum.  Administrators have the ability to edit
any content posted in the game, so choose wisely who you
are willing to allow as an administrator.

An administrator list is a list of username/password
pairs.  A typical list might look like this:

    `$Admins = Array(
        'admin' => 'passwd1',
        'john' => 'mypasswd',
        'hotdog' => 'pickles',
    );`

Set up any desirable additional defaults, like the skin
(template) or timeouts by editing the various settings
in the config file.  It's well-documented, so it shouldn't
be hard to change it.

## Step 3a. (optional) Set up the search engine.

The search engine allows anyone to quickly and easily
search the posted episodes by displaying a simple keyword-
search box.  It supports required and excluded flags (+ and
-) and generally operates the way a search engine should.

However, it uses some site features that may not be readily
available in your environment, and is not quite as easy to
set up as the rest of AG because it uses a cron job:  Once
per day, the search engine crawls through the database of
episodes and updates the search database according to what
it finds.  This results in very fast searches (a typical
search takes 2 milliseconds or less), but requires both
Perl and cron to work correctly.

To set up the search engine, first make sure you have Perl
properly installed on your server.  Then edit your crontab
to include a like like the following:

`0 4 * * * /<path to ag>/ag/calc_stats`

Make sure to substitute the real absolute path to AG where
you see <path to ag> above.  The line above will cause the
search to occur at 4:00 AM each day.  The search crawling
process will be niced so that it does not interfere with
the rest of your server's operations.

### Step 4.  Set up your start page.

Open "ag/admin.php" in your web browser.  You should see
a login page.  Log in using an administrator password and
you will see the (rather short) administrator's menu.
Click on the "Edit" button to edit episode #1.

All users will start on episode #1, so it's a good place
to specify your rules and regulations, what you allow and
don't allow on your site.  Some example rules are provided.
Save your changes and click "Go to the updated episode".

(If you have set up the search engine, now would be a good
time to initially populate its data.  Go back to
"ag/admin.php" and click on "Update statistics and search
engine" to run the search crawl for the first time.  It
will run fairly quickly, since there's almost no data for
it to look at.)

### Step 5.  Start your game!

At the bottom of episode #1, there will be a link titled
"Start the game".  Click on this link, and you'll be asked
to create episode #2.  Episode #2 is where you can put the
content of your story, where you can introduce your
characters or scenes, or where you can provide a launching
point to several additional possible stories.


## UPGRADE INSTRUCTIONS

If you already have an installation of AG and just want to
upgrade the software, the best way to do it is like this:

### Step 1.  (optional) Backup the old game!

Back up your current data/ directory and config.php files.
These are the only things that the game relies on to store
episodes and permissions.

    Example commands:
    `> tar -czvf mygame-backup.tar.gz data/* config.php`

You can optionally include the full AG directory if you
want to have a complete working backup of the existing
installation.

### Step 2.  Unpack the new tarball.

Unpack the new AG tarball to a separate directory.  Do
*not* unpack it over top of your current installation.

    Example commands:
    `> tar -xzvf ag-1.3.tar.gz`

### Step 3.  Delete the default data and templates.

Go into the new AG directory, and delete the data/
and templates/ subdirectories and "config.php".

    Example commmands:
   `> cd ag/
    > rm -rf data templates
    > rm -f config.php`

### Step 4.  Move the old data, templates, and settings over.

Copy or move your current data/ and templates/ directories
over to the new AG installation directory.  If you copy,
be sure to make sure everything in the copied data/
directory is writable by your web server.  Copy or move
your current "config.php" file over to the new AG
installation directory.

    Example commmands:
    `> cd ag/
    > mv ../mygame/data .
    > mv ../mygame/templates .
    > mv ../mygame/config.php .`

### Step 4a. (optional) Set up the search engine.

New to version 2.0 is the search engine, so if you're
running an old version, you may want to set up search.

The search engine allows anyone to quickly and easily
search the posted episodes by displaying a simple keyword-
search box.  It supports required and excluded flags (+ and
-) and generally operates the way a search engine should.

However, it uses some site features that may not be readily
available in your environment, and is not quite as easy to
set up as the rest of AG because it uses a cron job:  Once
per day, the search engine crawls through the database of
episodes and updates the search database according to what
it finds.  This results in very fast searches (a typical
search takes 2 milliseconds or less), but requires both
Perl and cron to work correctly.

To set up the search engine, first make sure you have Perl
properly installed on your server.  Then edit your crontab
to include a like like the following:

`0 4 * * * /<path to ag>/ag/calc_stats`

Make sure to substitute the real absolute path to AG where
you see <path to ag> above.  The line above will cause the
search to occur at 4:00 AM each day.  The search crawling
process will be niced so that it does not interfere with
the rest of your server's operations.

### Step 5.  Fix crosslinks.

The old 1.x versions of AG had a bug in handling crosslinks
(where the user specifies a specific target episode for a
link), in that the system would mark them the same as
normal links.  This has been fixed in 2.0, but you will
need to update the data in your database.  AG 2.0 includes
a script that can fix these bad crosslinks, and you should
run it now.

First, make a backup copy of your data/ directory!  This is
important in case (for some unlikely reason) something goes
wrong in the fixing job.

In your web browser, open AG's "fix-crosslinks.php" script.
This will walk through each episode, check to see if it
uses damaged links, and will repair them as necessary.  The
script is designed to handle sites that have episodes
numbered 5000 or smaller; if your site has larger-numbered
episodes, you will need to edit "fix-crosslinks.php" and
increase its analysis limit (the first line of the script).

If everything goes right, you should see nothing happen.

### Step 6.  Test the upgrade.

Open the new game in your web browser.  Everything should
work just as it did before, only better, since it's using
the upgraded version of AG.

### Step 7.  Swap directory names.

Rename your directories to move the old game out of the
way and move the new game into place.

    Example commmands:
    `> mv mygame oldgame
    > mv ag mygame`

### Step 8.  Delete the old game files (optional)

If everything works, you can safely remove the old game
directory now.

### Step 9.  Populate the search engine (optional)

If you have set up the search engine, now would be a good
time to initially populate its data.  Go to "ag/admin.php"
and click on "Update statistics and search engine" to run
the search crawl for the first time.


## CHANGELOG

Version 1.0 - Inker
  First private release.

Version 1.1 - Inker
  Added Smarty templating support.

Version 1.2 - Inker
  Added administrator login.
  Added administrator page-edit function.

Version 1.3 - Inker - August 2, 2006
  Added design documentation.
  Added recent-posts list to start page.
  Added installation instructions (this file).
  First public release.

Version 1.3.1 - Inker - August 2, 2006
  Fixed recent-list update bug when an episode is edited.
  Turned htmlentities() calls into htmlspecialchars() to
    correctly handle characters above #128.

Version 1.3.2 - Inker - August 3, 2006
  Fixed title "quotes" display bug in read.php.
  Fixed comma-encoding bug in the recent-episode list.
  Fixed default edit.tpl and create.tpl to correctly show
    how to use [img] BBCode tags.
  Fixed default 'next' value to be 3, not 2; this would
    cause nasty bugs in new installs :(
  Fixed blank-link bug; blank links cannot be created.

Version 1.3.3 - Inker - August 4, 2006
  Changed ClaimEpisode() in ag_core.php to prevent
    creation attacks, where a script could try to create
    many successive episodes to block creation of them
    altogether.  New code is much more thorough about
    keeping the lockfile clean, and prohibits a given IP
    from claiming more than one episode at a time.

Version 2.0 - Inker - March 7, 2007
  Major changes:
  Began process of converting over to a more class-based
    code-centralized design.
  Moved templates into a configurable template directory.
  Added the search engine and search crawl system.
  Added IP ban control.
  Added the ability to delete episodes (finally).
  Added administrator rating limits.
  Added announcement boxes.
  Added episode reporting and the report log.
  Fixed lots and lots of little miscellaneous bugs.
  Fixed cross-referencing links to use the correct '>' marker
    instead of the '+' marker.
  Improved default style template to highlight links.


## COPYRIGHT AND LICENSE

  Copyright (C) 2006-7 by the Phantom Inker

  This software is provided 'as-is', without any express or implied
  warranty.  In no event will the authors be held liable for any damages
  arising from the use of this software.

  Permission is granted to anyone to use this software for any purpose,
  including commercial applications, and to alter it and redistribute it
  freely, subject to the following restrictions:

  1. The origin of this software must not be misrepresented; you must not
     claim that you wrote the original software. If you use this software
     in a product, an acknowledgment in the product documentation would be
     appreciated but is not required.
  2. Altered source versions must be plainly marked as such, and must not be
     misrepresented as being the original software.
  3. This notice may not be removed or altered from any source distribution.

  Phantom Inker
  inker2576@yahoo.com
