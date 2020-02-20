git-recover: recover deleted files in your repository
===========

[![CI](https://github.com/ethomson/git-recover/workflows/CI/badge.svg)](https://github.com/ethomson/git-recover/actions?query=workflow%3ACI)

`git-recover` allows you to recover some files that you've accidentally deleted
from your working directory.  It helps you find files that exist in the
repository's object database - because you ran `git add` - but were never
committed.

Getting Started
---------------

The simplest way to use `git-recover` is in interactive mode - simply run
`git-recover -i` and it will show you all the files that you can recover
and prompt you to act.

Using git-recover
-----------------

Running `git-recover` without any arguments will list all the files (git
"blobs") that were recently orphaned, by their ID.  (Their filename is not 
known.)

You can examine these blobs by running `git show <objectid>`.  If you
find one that you want to recover, you can provide the ID as the argument
to `git-recover`.  You can specify the `--filename` option to write the
file out and apply any filters that are set up in the repository.  For
example:

    git-recover 38762cf7f55934b34d179ae6a4c80cadccbb7f0a --filename shattered.pdf

You can also specify multiple files to recover, each with an optional output
filename:

    git-recover 38762c --filename one.txt cafebae --filename bae.txt

If you want to recover _all_ the orphaned blobs in your repository, run
`git-recover --all`.  This will write all the orphaned files to the current
working directory, so it's best to run this inside a temporary directory
beneath your working directory.  For example:

    mkdir _tmp && cd _tmp && git-recover --all

By default, `git-recover` limits itself to recently created orphaned blobs.
If you want to see _all_ orphaned files that have been created in your
repository (but haven't yet been garbage collected), you can run:

    git-recover --full

Options
-------
    git-recover [-a] [-i] [--full] [<id> [-f <filename>] ...]

`-a`, `--all`  
Write all orphaned blobs to the current working directory.  Each file will
be named using its 40 character object ID.

`-i`, `--interactive`  
Display information about each orphaned blob and prompt to recover it.

`--full`  
List or recover all orphaned blobs, even those that are in packfiles.  By 
default, `git-recover` will only look at loose object files, which limits
it to the most recently created files.  Examining packfiles may be slow,
especially in large repositories.

`<id>`  
The object ID (or its abbreviation) to recover.  The file will be written to
the current working directory and named using its 40 character object ID,
unless the `-f` option is specified.

`-f <filename>`, `--filename <filename>`  
When specified after an object ID, the file written will use this filename.
In addition, any filters (for example: CRLF conversion or Git-LFS) will be
run according to the `gitattributes` configuration.

Getting Help and Contributing
-----------------------------
To report bugs, get assistance or provide a bug fix to this program,
check it out on [GitHub](https://github.com/ethomson/git-recover/).

Copyright (c) [Edward Thomson](http://edwardthomson.com/).  All rights reserved.

git-recover is open source software and is available under the MIT license.
Please see the included `LICENSE` file for more information.
