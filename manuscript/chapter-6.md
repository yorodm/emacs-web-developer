The DIY development environment
===============================

If you\'re from around this planet[^1] there\'s a big chance you\'ve
heard from something call IKEA[^2]

The checklist.
--------------

1.  How to manage local and remote files.
2.  Using Git with Emacs
3.  Running external tools or scripts.
4.  Working with SQL and databases.
5.  Working with virtualization technologies.

Managing files
--------------

`Dired` is Emacs built-int file manager, it allows us to forget about
switching to our system file explorer and easily do everithing without
leaving the editor.

Let\'s open a `dired` buffer by either typing `M-x dired RET` or `C-x d`

Working with direct is easy, here\'s a list of the most commonly used
commands.

-   `m` marks (selects) a file.
-   `u` unmarks a file.
-   `RET` opens a file or directory.
-   `^` goes to the parent directory.
-   `C` copies a file.
-   `R` renames (or moves) a file.
-   `D` flags a file for deletion.
-   `x` deletes all flagged files.
-   `Z` compresses the selected file.
-   `+` creates a new directory
-   `g` refreshes the dired buffer.
-   `h` show\'s the mode help page.
-   `q` leaves dired mode.

There are plenty more we can do, like using regular expressions to
select files or run a shell command in the marked files. There are also
a lot of extensions that make even easier to operate direct, my
favorites are `dired-single` to keep the mode from opening unnecesary
buffers every time I change directories and `dired-filetype-face` to use
different colors for filetypes. Here\'s an example of how to set them up

``` {.commonlisp org-language="emacs-lisp"}
(defun my-dired-init ()
  "Pimps my dired-mode"
  (define-key dired-mode-map [return] 'dired-single-buffer)
  (define-key dired-mode-map [mouse-1] 'dired-single-buffer-mouse)
    (with-eval-after-load 'dired  (require 'dired-filetype-face))
(define-key dired-mode-map "^"
  (function
   (lambda nil (interactive) (dired-single-buffer "..")))))

```

### Accessing remote files with TRAMP

TRAMP stands for *Transparent Remote (file) Access, Multiple Protocol*
and is basically a way to manipulate remote files from within Emacs by
using standard tools like OpenSSH or Putty.

But how exactly does this transparent access work? Let\'s say we want to
edit a file located in a remote server running Linux. In your pre-Emacs
days you would log into the server and open the file using whichever
editor the server got.

Same thing goes for `Dired` buffers

### FTP access.

Not all servers provide us with a remote account to work with, actually
most of the time all we have is a lousy FTP folder[^3]

[TODO]{.todo .TODO} Version control. {#version-control.}
------------------------------------

Revision control system\'s are like opinions, everybody has the best
one, although in my case the best one happens to be `Git`. `Magit` is an
Emacs mode to interact with `Git` allowing even experienced users to
perform their daily tasks directly from within Emacs[^4]. In the words
of it\'s creator Jonas Bernoulli, Magit aspires to be a complete Git
porcelain

We can get `Magit` from either `Melpa` or `Marmalade` or by typing the
usual `M-x package-install RET magit RET` sequence, Once installed, you
can start it with the command `magit-status`. Since there\'s a big
chance you\'ll be doing that every 10 minutes, let\'s create a key
binding for it.

``` {.commonlisp org-language="emacs-lisp"}
;; Binds C-x g to the command magit status
(global-set-key "\C-xg" 'magit-status)
```

There is also a `magit-clone` command that allows you to clone an
existing repository without leaving the editor.

![Magit status
buffer.](./images/emacs-magit-status.png "emacs-magit-status")

Operating `Magit` is easy, since it\'s one of those *special buffers*
there\'s no need to learn complicated commands and almost everything can
be accomplished with three keystrokes.

The basic commands are:

-   `h` or `?` help pop up windows.
-   `TAB` expands/collapses a section.
-   `l` shows git\'s log.
-   `d` diff files
-   `s` will stage a file (or changes).
-   `u` unstage both files or changes.
-   `z` creates a stash.
-   `k` discard changes.
-   `c` commits, it also launches an special buffer for you to write the
    commit message.
-   `b` manage branches.
-   `m` merges.
-   `g` refreshes the status buffer.
-   `q` quits magit (closes the buffer).

> You can learn more about working with Magit in the official
> documentation. Check it in the info documentation browser.

[TODO]{.todo .TODO} Running external commands {#running-external-commands}
---------------------------------------------

1.  Shell- mode
2.  commint-mode

SQL and more.
-------------

The SQLi (as in *SQL interpreters*) commands are specialized `comint`
routines to work with various database clients. The list is quite
extensive and includes the most popular free and propietary database
servers. Each command has the prefix `sql-` followed by the database
vendor\'s name and requires us to provide connection details like
database name and credentials, so if we wanted to start a new PostgreSQL
session we would enter `M-x sql-postgres RET` and then fill the user,
database and server part, for other products additional info like
passwords may be required.

Each of the SQLi commands can be customized by setting the variables, I
recommend setting this with extreme care specially since some of them
are shared for all commands:

-   `sql-<vendor>-program` (the client program)
-   `sql-user` (default user name)
-   `sql-password` (duh!)
-   `sql-database` (the database name)
-   `sql-host` (the host name or IP address)
-   `sql-port` (the server\'s port number)

We can also create *connection profiles* using the
`sql-connection-alist` variable and use the `sql-connect` command to
rapidly access a given profile. As a security measure it is best not to
save passwords (not even in your `init.el`)

``` {.commonlisp org-language="emacs-lisp"}
(add-to-list 'sql-connection-alist
             '("dev-server"
              (sql-product 'mysql)
              (sql-user "wordpress")
              (sql-database "wordpress")
             (sql-server "127.0.0.1")))
```

Profiles can also be created calling `sql-save-connection` on an open
SQLi buffer.

IDE-in-a-box
------------

The Emac\'s community is a nonstoping hive of working bees. Every now
and then someone creates and distributes a configuration kit to make
things easier for newbies and experts users alike.

Footnotes
=========

[^1]: If you\'re not, the crazy guy from upstairs we\'ll be glad to know
    you.

[^2]: IKEA is a trade mark from

[^3]: No disrespect to FTP intended

[^4]: DEFINITION NOT FOUND: fn:268ffbea
