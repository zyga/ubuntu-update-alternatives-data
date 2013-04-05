Ubuntu update-alternatives Data
===============================

This repository contains a automatically created and manually verified database
of how each package in Ubuntu (percise, quantal, raring) is using the
update-alternatives command.

The intent of this database is to help build reliable data for
command-not-found which suggests which packages contain a given command
executable

The long term solution is to move this data right into each package. Preferably
into the control file itself.

How to generate this data?
==========================

This data is generated manually, by confirming automatic suggestions and fixing
things that were not handled automatically with manual edits.

To generate the data you need a full copy of the ubuntu archive (easily
obtainable with debmirror) and the check-update-alternatives script from the
command-not-found repository
(https://github.com/zyga/command-not-found/tree/check-update-alternatives).

Run the command as follows:

    ./check-update-alternatives /path/to/archive update-alternatives-db.ini

And answer questions as they are asked. Note that it is safe to interrupt the
program while a question is being asked. All previous answers are retained
and written back to the database.

Hot to interpret the data?
==========================

The database is saved as a windows-style ini file. Sections represent distinct
records. The name of each section is composed of the name and version of a
package. Since Debian package versions cannot be reused it is safe to assume
that each record uniquely identifies a particular binary package.

The most important data stored in each record is the ``alternatives`` key. If
present, it contains a space-separated list of pairs link=path that enumerate
all usages of update-alternatives of a given package.

The presence of the ``manual-check == yes`` key-value pair indicates that the
package needs to be inspected manually. This key should be removed after
inspection.

The presence of the ``using-hint = yes`` key-value pair indicates that this
record was derived from a previously validated guess. Technically the guess is
based on hints, not on the postinst script so this _might_ be incorrect but
usually is not. 

The presence and value of the ``hint-correct`` key encodes if the automatically
generated list of hints was correct or not, as determined by manual inspection.

The presence of the ``hint-hash`` key indicates the hash of the canonical
representation of the list of hints. This is used as an implementation detail,
to accelerate discovery of hints that can be reused. See the source code for
details on how this is computed and used. 

The presence of the ``using-hint = yes`` key-value pair indicates that this
record reused an answer from another record and that it was not manually
verified.
