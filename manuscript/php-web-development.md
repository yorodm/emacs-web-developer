The PHP chapter
===============

You may love it, you may hate it, but you certainly can\'t ignore it.
PHP has been one of the major players in the web development scene since
the day it came to life[^1]. It is virtually everywhere, from blog
engines, to forum sites, to Facebook.

Back in the day when I decided to write all my PHP code in Emacs, people
told me to use `php-mode`. I went along for a few days because it seamed
like the obvious choice: it had syntax highlighting, a command-based
snippets system and support for a lot of *electric* features. Then I
found it didn\'t support working with PHP/HTML files (I was working in
my first Wordpress theme at the time), that left me with no choice but
to switch between `html-mode` and `php-mode` every now and then. No
good.

But that was *ages* ago, now I\'ve moved pass both `html-mode` and
`php-mode` and found a new way: `web-mode`. Instead of focusing on one
particular language/techology it provides us with a nice way to code web
template files so... no more switching. We\'ll also need a way to handle
all those pesky things like snippets, syntax checking and completion and
navigation. Let\'s begin.

The checklist.
--------------

So what do we need to turn Emacs into the ultimate PHP editor? As usual
let\'s make a checklist:

1.  Pick a mode for the different styles of PHP development.
2.  Make some tweaks to help with your code
3.  Make use of existing tools.

DIY PHP mode.
-------------

Now let\'s take a critical look at `web-mode`, besides nice syntax
highlighting there are no actual PHP related features. What do we do
with our PHP snippets? What about syntax checking?. We already now that
modes can be customized by setting variables or adding function hooks so
you might feel tempted to create something like this:

``` {.commonlisp org-language="emacs-lisp"}
(add-hook 'web-mode-hook
          ;; A hook to activate PHP features
          (lambda ()
                  (set-my-syntax-checking-thingy)
                  (yas-activate-extra-mode 'php-mode)))
```

But...there\'s a problem, this hook function will run even if when
we\'re not editing a PHP file. Okay, we can solve this, just need to
figure out some kind of file type detection mechanism then we could
create hooks that will only work witch centain file types; except, that
sounds a lot like creating a new mode.

In a previous chapter we talked about derived modes as a way to create
new major modes by *inheriting* an existing one. Since all we want is to
make some changes around how `web-mode` works, we can do this:

``` {.commonlisp org-language="emacs-lisp"}
(when (fboundp 'web-mode) ;; if web-mode is installed
   (define-derived-mode php-web-mode web-mode "php-web-mode"
      "A web mode for php."
      (emmet-mode) ;; enable emmet for extra fun!!
      ;; also enable php-mode snippets
      (yas-activate-extra-mode 'php-mode))
;; further configuration goes here
(add-to-list 'auto-mode-alist '("\\.php[s345t]?\\'" . php-web-mode)))
```

Now instead of a hook, we have a full fledged `php-web-mode`, it
features all of the things we loved about it\'s parent plus snippets and
emmet style abbreviations.

### Mo\' modes mo\' fun.

Ok, we\'ve already stablished that you\'re a PHP developer but, what it
is exactly what you do? Let\'s say that most of your work involves
developing Wordpress plugins but you\'re also creating a brand new
Symfony based CMS on the side. Thing is you don\'t want the Wordpress
and Symfony snippets to mix up every time you open a PHP file. The best
solution is to create a major mode for every case and set specific
snippets for each one.

``` {.commonlisp org-language="emacs-lisp"}
(define-derived-mode wp-mode web-mode "wp-mode"
   "A major mode for editing wordpress files"
;; Same old, same old
)
(define-derived-mode wp-mode web-mode "symfony-mode"
   "A major mode for working with symfony"
;; Same old, same old
;; also some comint stuff to work with symfony commands
)
```

Having an specific mode for every case may seem like too much but
believe me, it is the smartest way to keep all your settings in check.
We could just create some hooks and set some variables but soon we\'ll
be needing more hooks and more variables and then we\'ll be drowning in
a lake of unmaintainable code. Let\'s see a hypothetical example about
wordpress and symphony development

``` {.commonlisp org-language="emacs-lisp"}
(define-derived-mode wp-mode web-mode "wp-mode"
   "A major mode for editing wordpress files"
   (setq tab-width 4) ;; tabs are 4 spaces wide
   (set-up-less-and-recess-for-css-compialtion)
   (setq indent-tabs-mode t)) ;; and use hard tabs please

(define-derived-mode symfony-mode web-mode "symfony-mode"
   "A major mode for editing wordpress files"
   (symfony-comint-functions)
   (setup-rest-client-in-a-window-below)
   (setq tab-width 3) ;; tabs are 4 spaces wide
   (setq indent-tabs-mode nil)) ;; and use soft tabs please
```

As a bonus remeber we can create mode hierarchies, not much unlike class
hierarchies in OOP.

``` {.commonlisp org-language="emacs-lisp"}
(define-derived-mode php-web-mode web-mode "php-web-mode"
   "A web mode for php."
  ;; Common stuff
   )
(define-derived-mode symfony-web-mode php-web-mode "symfony-web-mode"
   "A web mode for php."
  ;; The usual stuff
   )
(define-derived-mode wp-web-mode php-web-mode "wp-web-mode"
   "A web mode for php."
  ;; The usual stuff
   )

```

I usually put all these into a `custom-modes.el` file

On the fly.
-----------

I\'m rather fond of PHP Code Sniffer for syntax and coding standars
checking. Emacs provides the package `flyphpcs` as way to bring the
power of this tool into our development process. Setting it up can\'t be
easier, install the required package and add these lines to our
`php-web-mode` definitions.

``` {.commonlisp org-language="emacs-lisp"}
(setq flymake-phpcs-command "/path/to/phpcs") ;; path to executable
(setq flymake-phpcs-standard "PSR2") ;; coding standard
(flymake-mode)
```

Now we can have those squiggly red thingy under our syntax errors. We
can also get visual notification if our code doesn\'t follow a selected
standard, I usually set this to `PSR2` but you may choose `Zend`, `PEAR`
or `PSR1` among others.

If you\'re a Windows user remember to create a `.bat` file to run the
command:

``` {.bat}
@echo off
php phpcs.phar %*
```

Navigating through code.
------------------------

One of the great things about free software is the possibility to
explore other people\'s code and learn about their work (or write code
reviews). Most IDE\'s include code navigation to some extend but it is
often a missing feature in code editors. Emacs has it\'s own approach to
the matter, tag tables.

A *tag table* is an index file for source code, it allows us to find a
function, class or variable anywhere in our project without having to
remeber in which file it was defined. Emacs provides the `etags`
facility to create such index files and allowing not only navigation but
also `isearch` and `query-replace` accross multiple files.

> `Etags` version 24.5 (wich at the time I wrote this section was the
> last stable release) lacks support for some of the \"latest\" PHP
> features like traits, namespaces, and interfaces. There are far better
> solutions to index and navigate PHP code but since this is a book
> about Emacs it felt wrong not to talk about `Etags`.

`Etags` is also the name of the command line utility that parses source
code and indexes it\'s contents into a `TAGS` file[^2], one bad thing
though: `Etags` doesn\'t support searching throught directories so to
index our whole project we may have to do something like this:

``` {.shell}
# Go to the root of your project and execute this

# *nix version
find . -name "*.php" -exec etags -l php  -a {} \;

# Windows version using Gnu On Windows
gfind . -name "*.php" -exec etags -l php  -a {} ;
```

Now we need to tell Emacs which `TAGS` file are we going to use for
navigation with the command `visit-tags-table`. After that we can
navigate with `M-.` or `find-tag` and return to our previous position
with `M-*` or `pop-tag-mark`.

> There are four other commands in the `find-tag` family. They allow you
> to search a tag using regular expressions, display the search result
> in another window or frame and search for a tag without changing your
> current buffer. I dare you to find their names.

Code completion
---------------

Can we live without code completion? Of course we can, but, do we want
to? Before you answer the question let me introduce you to `ac-php`,
Emacs solution to having bad memory for classes and functions\'s names.
To use this mode we\'re going to need a few extra utilities like
`cscope` and I\'m asumming you already have a PHP command line
interpreter[^3] installed or at least know how to get one for your
OS[^4].

We\'re now going to get `ac-php` form Melpa and configure the editor so
it activates completion every time you open a PHP file. If you followed
my advice on derived modes then adding a hook to the root of the
hierarchy is enough

``` {.commonlisp org-language="emacs-lisp"}
(add-hook 'php-web-mode-hook
          '(lambda ()
                   (auto-complete-mode-t)
                   (ac-sources '(ac-source-php))))
```

or if you are a `company` man

``` {.commonlisp org-language="emacs-lisp"}
(add-hook 'php-web-mode-hook
           '(lambda ()
                    (company-mode t)
                    (add-to-list
                        'company-backends
                        'company-ac-php-backend)))
```

### Dude, where\'s my class?

But how do the completion process actually works? Sadly configuring
`ac-php` is not as simple as installing the package and start typing.
The completion engine needs to scan our source files and create a
database of every variable, class and function in our code. For that to
happend we need to create a file called `.ac-php-conf.json` in the root
of our project and initialize the database with either
`ac-php-remake-tags` or `ac-php-remake-tags-all` usually from a Dired
buffer, since we\'ll be running those commands a lot (the database
doesn\'t update itself automatically) it\'s best to map at least one of
them to a key sequence.

``` {.commonlisp org-language="emacs-lisp"}
;; Add this to our mode definition
(define-key php-web-mode-map (kbd "<f5>") 'ac-php-remake-tags)
```

The `.ac-php-conf.json` file controls which files are indexed into the
database, when empty it defaults to:

``` {.json}
{
  "use-cscope" : true,
  "filter": {
    "php-file-ext-list": [
      "php"
    ],
    "php-path-list": [
      "."
    ],
    "php-path-list-without-subdir": []
  }
}
```

The `filter` object holds the properties we need to configure how the
completion database will be populated.

-   `php-file-ext-list` : Only search files with these extensions[^5].

-   `php-path-list` : Recursively search into these directories

-   `php-path-list-without-subdir`: Search these directories without
    recursing into any of it\'s subdirs.

Here\'s an example from a fictitious Symfony based project:

``` {.json}
{
  "use-cscope": false,
  "filter": {
    "php-file-ext-list": [
      "php"
    ],
    "php-path-list": [
        "./src",
        "./vendor/symfony/symfony/src",
        "./vendor/doctrine/orm/lib"
// Any additional directories
    ],
    "php-path-list-without-subdir": []
  }
}
```

That will index everything under `./src` (a.k.a your bundles) plus
Symfony\'s and Doctrine ORM\'s source creating a small database file
thus having less impact on performace[^6].

### Code navigation (reloaded).

Another feature of `ac-php` is code navigation,
`ac-php-find-symbol-at-point` uses the same geenrated information to
locate any symbol definition and jump to the proper file, we can move
back with `ac-php-location-stack-back`. I map these commands to `C->`
and `C-<`

``` {.commonlisp org-language="emacs-lisp"}
(define-key php-web-mode-map  (kbd "C->") 'ac-php-find-symbol-at-point)
(define-key php-web-mode-map  (kbd "C-<") 'ac-php-location-stack-back)
```

Both completion and navigation can take advantage of PHP Doc style tags
in our code, take the example below, here `$service`\'s type will be
correctly identified by the engine as `SuperServiceInterface`.

``` {.php}
class Whatever {
/**
 * @var SuperServiceInterface
 */
public $service;
```

Refactoring made easy.
----------------------

The PHP Refactoring Browser[^7] is a tool that provides a set of
commands to refactor PHP code. It\'s creator blah blah blah blah.

To get started download the script from blah blah blah. The mode also
uses the `patch` program to apply the refactors back into the
buffer.[^8]

At the time this chapter was written, there were five possible
refactorings:

-   Convert local to instance variables
-   Rename local variables
-   Extract method
-   Optimizes `use` statements in a file.

To apply any of those to our code we need to enter the prefix sequence
`C-c r` and then ask for an specific command

-   `lv` convert local to instance.
-   `rv` rename local variable.
-   `em` extract method (requires a region to be selected).
-   `ou` `use` statement optimization.

Refactorings are atomic operations. So if you don\'t like the results,
go with `C-x u` and forget it ever happended.

Add these lines to `php-web-mode`

``` {.commonlisp org-language="emacs-lisp"}
;; in php-web-mode-definition
 (setq php-refactor-command "/path/to/refactor.phar")
 (php-refactor-mode)
```

Again Windows users should create a `.bat` file for running the script.

``` {.bat}
@echo off
php path\to\refactor.phar %*
```

Testing and more.
-----------------

I\'m a big TDD fan so having a way to easily run tests is very important
to me. There are many test frameworks for PHP but since PHPUnit is the
de facto standard I\'ll be focusing this segment on how to work with it.

First we\'ll be downloading PHPUnit[^9] and installing the package
`phpunit` from archives, to make things easier Windows users should
create a `.bat` file.

``` {.bash}
@echo off
php path\to\phpunit.phar
```

The `phpunit` Emacs library provides us with a set of commands to run
tests:

-   The `phpunit-current-tests` command launches the test currently in
    our buffer.
-   The `phpunit-current-class` searches for a test matching the name of
    the class currently in our buffer.
-   Finally `phpunit-current-project` will run every test for the
    current project.

Of course for those commands to work we need to properly set up PHPUnit
and have some respect for things like coding conventions, but that\'s a
topic for another book.

Tools of a trade
----------------

Sadly there are to many utilities and not enough modes. Here are some
recipes to make interactive Emacs buffers for some of my favorites PHP
utilities, let\'s say we want to run PsySH[^10] which is kind of my
favorite PHP shell out there.

``` {.commonlisp org-language="emacs-lisp"}
(defun run-psysh ()
  (let* ((composer-root (getenv "COMPOSER_HOME"))
         (command-path (or composer-root "")))
    (make-comint "PsySH"
                 (concat command-path "/vendor/bin/psysh"))))
```

I also like to keep an interactive Symfony console instead or running
commands one at a time.

``` {.commonlisp org-language="emacs-lisp"}
;; Add this to your init.el file
;; Linux users can just (make-comint "name" "/path/to/app/console")
(setq symfony-projects-path "/path/to/symfony/projects/")
(defun symfony-run-shell (project)
   (interactive "sProject:")
   (make-comint "Symfony Console"
                "path/to/php.exe" nil
                (concat symfony-projects-path
                        project "/app/console") "-s"))
```

The same can be used for Sylex, Laravel and such.

Footnotes
=========

[^1]: Some people may refer to this as \"ye olde days\".

[^2]: I bet you didn\'t saw that one coming.

[^3]: Go to a terminal and type `php`, if nothing happends then you\'re
    in trouble

[^4]: By the way, `ac-php` only supports \*nix systems like Linux or OSX
    so... no Windows.

[^5]: Some frameworks use weird file extensions like `.inc`.

[^6]: Full disclosure: There\'s no databse, just a bunch of `.el` files
    with some huge association lists in them. Those files will be loaded
    and executed as normal Elisp code does.

[^7]: <https://github.com/QafooLabs/php-refactoring-browser>

[^8]: `patch` should be included in your \*nix system. Windows users can
    get Cygwin, Gow, MSYS or whatever they like.

[^9]: <https://phar.phpunit.de/phpunit.phar>

[^10]: <http://psy.org>
