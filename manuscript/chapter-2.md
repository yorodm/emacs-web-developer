Emacs 101.
==========

Welcome to **Emacs For Web Developers** an almost serious attempt to
write a book about using GNU Emacs as a working environment.

What\'s with this Emacs thingy?
-------------------------------

In the words of it\'s creators, Emacs is an extensible, customizable,
self-documenting editor. For us users it\'s the perfect working
environment capable of running shell commands, perform complex tasks,
browse web sites and even check our mail. Emacs has also a reputation
for being an extremely complicated piece of software which is not
entirely true.

The first version of Emacs was created by Guy Steele and Richard
Stallman[^1] around 1976 as a set of macros for TECO[^2]. In the
following years a bunch of commercial versions pop into the market until
Stallman and the Free Software Foundation released version 13[^3] of GNU
Emacs in March 1985. Thirty years later I\'m writing this book using
version 24.5 running under Microsoft Windows 8.1.

![GNU Emacs 24.5 running under Windows
8.1](./images/emacs-first.png "emacs-first-run")

In this chapter I\'ll give a quick introduction to the editor itself and
see what else we need to have a proper working environment.

First steps with Emacs
----------------------

In this book we\'re going to talk mainly about GNU Emacs although
Aquamacs (a Mac OSX version of the editor) users should be fine with
everything explained here too. Like I said GNU Emacs is free (as in
freedom) software so it\'s freely distributed under the terms of the GPL
and if you\'re using Linux there\'s a big chance there\'s already some
pre-installed version with your system. In any case you can just
download the latest version available at the [GNU
website](http://www.gnu.org/software/emacs).

When you run GNU Emacs for the first time you\'ll be seeing something
like \hyperref[fig:emacs-first-run]{Figure 1.1}. That is Emacs initial
window, it displays some weird logo and a bunch of text with links to
different help entries. The window itself is divided into five sections
as shown by

1.  The menu bar.
2.  The default UI tool bar.
3.  The buffer window (With the logo and all the weird text)
4.  The mode line (The gray thingy).
5.  The minibuffer window (Below the gray thingy).

One of the first thing we need to know about working in GNU Emacs is
that it was created in an era before graphical user interface became
popular so even if there\'s a menu bar most of the editor\'s
functionality is available **only** through key sequences instead of
your usual \"click on the menu\" way. The second thing we need to be
aware of is that even that can be changed, when it comes to customizing
Emacs nothing\'s beyond our reach.

Some basic terminology
----------------------

Before we continue on our journey there are some terms that will be
thrown around in this book and you should know more about their meaning.

### Frames

In Emacs lingo, the system window is called a \"frame\". You can have
multiple frames linked together in a single Emacs session so any changes
you make in one of them will be shared by the rest. A frame can be
divided into multiple windows.

### Window

A window is the section inside a frame responsible for displaying a
buffer and it\'s mode line. Each window can display one and only one
buffer at a time. The same buffer (or sections of it) can be shown by
more than one window. At any time, one Emacs window is the selected
window and it\'s buffer is the current buffer. When you move or change
your cursor position in one window it doesn\'t affect other windows,
even if they\'re displaying the same buffer.

### Buffers

Every time you open a file, read your mail or display help about
something a new buffer is created. Buffers are mostly used for holding
and displaying text (and a bit of information about it). When you open a
file, Emacs reads it\'s contents into a buffer. Each buffer has an
unique name and only one will be active at any time, that\'s referred to
as the \"current buffer\".

### Modes

Editing modes are code libraries designed to alter the editor\'s basic
behavior in some useful (sometimes funny) way. There are \"major modes\"
and \"minor modes\". Major modes provide special features usually
related to he kind of file you\'re editing but some are used by Emacs
special buffers (like the one that checks your mail). At every time a
buffer can have one and only one major mode at the time. On the other
hand minor modes are optional features which you can turn on and off as
needed, they\'re independent of one another and of the selected major
mode.

### Mode Line

The gray thingy nearly at the bottom of the window. It describes what\'s
going on in the current buffer and of course you can tweak how much and
what information is displayed. The default mode line shows something
like

``` {.example}
U(Unix)** emacs-101.org Top L22 (Org yas Fill)
```

Which is a very cryptic way to say: \"The current buffer uses UTF-8
encoding, has Unix line endings, has been modified (the \*\* part) it\'s
file name is `emacs-101.org`, we are positioned at line 22 counting from
the top of the buffer. Finally the buffer major mode is called **Org**
and we also have two minor modes called **yas** and **Fill**\".

### Echo area and Minibuffer

The echo area is always located at the very bottom of the editor frame.
It is used to display small informative messages and \"echoing\" the
characters of any multi-character command you type, error messages are
also displayed in the echo area. If you miss a message don\'t worry,
there\'s an special buffer called `*Messages*` that keeps track of every
one of them. The echo area is also home of the minibuffer. An special
window where you can execute commands, input arguments, etc.

### Commands

Commands are special editor routines, each one has a name (and sometimes
and alias) so you can execute them from the minibuffer entering
`M-x <name of the
command> RET`. Since typing that every time is quite a nuisance commands
can be bound to short sequence of keystrokes called *key sequences*.

### Key sequences

Key sequences are the preferred way of getting things done in all
Emacsen (even Aquamacs). Take for example `C-x C-s`. That\'s the default
key sequence for the command `save-buffer` and it means \"press
`Control+x` and then press `Control+s`\". This notation has become the
standard way to describe key sequences so I\'ll be using it *a lot*
throughout the book.

-   `S` stands for the `Shift` key on your keyboard
-   `RET` stands for `Enter` or `Return`
-   `C` stands for `Control`
-   `M` stands for `Alt` [^4] or `Esc`.
-   `SPC` for the space key
-   Any other symbol stands for itself.

There are about a hundred standard sequences and since every mode can
add new ones to the list it is almost impossible to write them all
down[^5] so I will leave you with a few need-to-know sequences

-   `C-x C-f` Opens (visits) a file. If the file does not exist then it
    is created.
-   `C-x C-s` Saves the current buffer.
-   `C-x C-b` Switches buffers (requires user input)
-   `C-x C-k` Kills (closes) a buffer (requires user input).
-   `C-s` Searches forward (case insensitive)
-   `C-r` Searches backwards (case insensitive)
-   `M-x` Executes commands by name.
-   `C-g` Cancels any running command.
-   `C-x 2` Splits the window horizontally
-   `C-x 3` Splits the window vertically
-   `C-x 1` Closes all other windows
-   `C-x 0` Closes this window
-   `C-x C-c` Exits the editor.

> Mac OSX users should read their editor\'s documentation about how to
> map some of these keys to the Mac keyboard.

### Macros

If you discover that you\'re about to type the same combinations of
commands for the tenth time then is about time for you to start using
macros. A keyboard macro is an user defined command that stands for
another sequence of keys. That means everything from writing text to
creating files, you just have to recognize when you\'re doing repetitive
work and Emacs will take care of the rest.

### Packages

A package is a piece of code designed to extend Emacs behavior in some
way. From visual themes to running and embedded HTTP server, packages
are a big part of what makes Emacs great!.

[TODO]{.todo .TODO} Getting Help (like a pro) {#getting-help-like-a-pro}
---------------------------------------------

When in doubt always ask an expert and in this case, our resident expert
is the editor itself.

Emacs has a powerful built-in help system that covers everything from
simple key sequences to internal editor functions.

-   **C-h f \<function-name\>**: Find the key binding corresponding to
    \<function-name\> (ex: C-h f save-buffer)
-   **C-h k \<key-sequence\>**: Find the function name corresponding to
    \<key-sequence\> (ex: C-h k C-x C-s)
-   **\<key-sequence\> C-h**: Show the next possible keys to press after
    the \<key-sequence\> (ex: C-x 4 C-h)

When executing these commands, a new frame opens. To close it, switch to
it (*C-x o*) and type *q*. If not, simply close it (*C-x 0*)

After executing a command with M-x, if there\'s key sequence bound to
it, a message indicating what to press will show in the minibuffer.

Emacs also includes the official manual in GNU Info format (also
available on line:
<http://www.gnu.org/software/emacs/manual/html_node/emacs/>)

-   **C-h r**: browse the Emacs manual within Emacs

Tools for the trade
-------------------

Since this is a book about web developing, we\'re going to need more
than just a text editor. Depending on your programming language of
choice you should download:

-   `Git`, I\'m running version `2.5.2` but you should be fine with
    everything over `2.0`.
-   `Python`, either `2.7` or `3.X` should be fine.
-   A fairly decent version of `Nodejs` (`0.12.0` or above).
-   `PHP 5.4` or above.
-   `Gow` from their [home page](http://wiki.github.com/bmatzelle/gow/)
    (Windows users only).

In Microsoft Windows there\'s a little problem about where the
initialization file is located. This is due to Windows and Emacs having
a different understanding of where the user `HOME` directory is. There
are many ways to circumvent this issue. The easiest way is to set an
environment variable called `HOME` and point it to
`C:\Users\<your user name>`. Ok, now we are ready.

Footnotes
=========

[^1]: You know, the guy with the beard.

[^2]: What\'s with this TECO thingy?

[^3]: Does someone know what happened with versions 1 to 12?

[^4]: Actually it stands for \"Meta\" but they don\'t have those in
    keyboards anymore.

[^5]: Either that or I\'m to lazy to do it.
