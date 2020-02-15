# Command Line Study Guide

This is a guide for becoming more familiar with the command line. These are some basic GNU/Linux terminal commands that are useful to know as a minimum.

## Built-in Documentation

Most commands come with documentation in the form of "man[ual] pages". To read the man page for a command use the [`man` command](../terminal/man.md).

Example: to read the documentation for the `ls` command, type `man ls` in a terminal. You can also search Google for information on any command.

If the `man` pages are too overwheming, there is also a simpler tool called [tldr](../terminal/tldr.md).

## List of Useful Commands

Click on the name of a command for more information.

### Basics

* `clear` -- clear the terminal. Also ctrl-l
* `man` -- read the built-in documentation
* `tldr`
* `apropos` -- can't remember a command? Use this to find commands about a keyword, like: `apropos wireless`

### Where Am I?

* `pwd` -- show your current location in the filesystem
* `cd` -- change directory
* [`ls`](/terminal/ls.md) -- list directory contents. Also `ls -l`,  `ls -R`, and  `ls -al`.
* `tree` -- list a directory as a tree. e.g., `tree -d >> outputfile.txt`. You may need to install it first.

An alternative way to move around and keep track of where you've been is to use `dirs`.

* [`dirs`](../terminal/dirs.md) -- a stack of recently visited directories
* `pushd` -- move to another directory with a bookmark (actually a stack of directories you've jumped from, so you can use it multiple times)
* `popd` -- jump back to the place where you pushd'd from

### Manipulating Files

* `cp` -- copy files
* `mv` -- move a file or directory. Also for renaming things
* `mkdir` -- make a directory
* `rm` -- remove a file. Also `rm -rf`, but very dangerous. See [this story](https://www.theregister.co.uk/2017/02/01/gitlab_data_loss/) for a warning on how dangerous it can be.
* `rmdir` --- remove a directory
* `touch` -- create a new empty file

### Finding Files

* `grep` -- search the contents of files
* `rg` -- ripgrep
* `locate` -- find files
* `find` -- find files. E.g., `find / -name '*.desktop'`
* `fd` -- a find command with some improvements

### Programs

* `which`

### Networking

* `ping`
* `dig`
* `traceroute`

### Reading and Editing Files

* `less` -- display output with pagination
* [`vim`](/guides/vim.md) -- type `vimtutor` and see the [[Vim]] page.
* `nano` -- simple console editor
* `cat` -- display a file and/or concatenate it
* `tee` -- redirect the output to a file and the screen at the same time. E.g., `ls -1 *.py | wc -l | tee count.txt` which counts the number of Python files in your directory, writes it to the screen, and saves it to a file.
* `wc` -- count things: lines, bytes, characters, words, etc. Example: `wc -l filename.txt` will count the lines in a file.
* `head`
* `tail`
* `diff`

### Users, Groups, and Permissions

* `chmod`
* `chown`

### The System

* `top` -- show processes. If you like that, install `htop`.
* `htop`
* `du`
* `df`
* `kill`
* `ps`
* `shutdown`
* `reboot`
* `uptime`
* `date`
* `sleep`

### Compressing and Extracting Files

* `tar`
* `zip`
* `gzip`

## Other Tips

* tab completion
* pipes `|`, `>`, and `>>`
* aliases
* ctrl-r -- reverse search
* keyboard shortcuts: ctrl-u, ctrl-k, ctrl-a, ctrl-e, alt-f, alt-v, ctrl-d, alt-d (from Emacs)

For managing remote servers: [ssh, scp, and rsync](https://forum.codeselfstudy.com/t/useful-command-line-tools-for-managing-files-on-remote-servers/1041)

And [Tmux](https://www.ocf.berkeley.edu/~ckuehl/tmux/).

## Additional Utilities

Some of these may need to be installed.

* `sort` -- sorts items
* `uniq` -- gets only unique items
* `mc` -- Midnight Commander file browser
* `tr` -- translate
* `fold` -- wrap lines to a specified width
* `jq` -- tools for JSON
* `curl` -- do stuff with URLs
* `wget` -- download pages and sites
* `sql2csv` (`npm install -g sql2csv`)
* `csvkit` (`pip install csvkit`)
* [xml2json](https://github.com/hay/xml2json) (`git clone` it and add to path)
* ImageMagick -- process and view images, e.g., `display cat_pic.jpg`, `convert --resize 200x200 giant_hubble_photo.jpg hubble_photo_thumb.jpg`
* `rename` -- bulk rename files with regular expressions. Example: rename all files with the extension `.GIF` to `.gif`: `rename -v 's/\.GIF$/\.gif/' *.GIF`
* `lynx` -- a browser in your terminal.
- [GNU parallel](https://www.gnu.org/software/parallel/parallel_tutorial.html) ([cheatsheet](https://www.gnu.org/software/parallel/parallel_cheat.pdf))

You will occasionally come across these:

* `sed` -- stream editor for filtering and transforming text
* `awk` -- pattern scanning and processing language
* [Perl](http://www.math.harvard.edu/computing/perl/oneliners.txt) [one](http://www.catonmat.net/blog/introduction-to-perl-one-liners/) [line](https://blogs.oracle.com/ksplice/entry/the_top_10_tricks_of) [scripts](http://www.catonmat.net/download/perl1line.txt)

See additional tools that you might want to investigate:

https://forum.codeselfstudy.com/t/cli-improved-command-line-tools/1001

How to use the terminal for everything:

https://forum.codeselfstudy.com/t/using-the-terminal-for-everything/1005

`tig` can be used as an alternative to `git log`. `ranger` is a file browser.

https://forum.codeselfstudy.com/t/interesting-command-line-tools/112

## Keybindings

See this post for useful keyboard shortcuts:

https://forum.codeselfstudy.com/t/becoming-faster-with-the-command-line/990

## Additional Resources

See also [7 command line tools for data science](http://jeroenjanssens.com/2013/09/19/seven-command-line-tools-for-data-science.html).

See also [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/coreutils.html).

There are additional posts here: #command-line.
