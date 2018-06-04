The (almost) Elisp chapter
==========================

I won\'t be teaching you how to create Elisp programs in this book,
mainly because:

1.  Is a really long subject (the FSF reference manual has over 1000
    pages).
2.  You\'re all very busy people with better things to do.
3.  Some people spend years working with Emacs without knowing or caring
    about Elisp.

But, given that we\'ll be tweaking around the editor *a lot* in this
book I\'ll give you a really quick (and dirty) introduction to the
language and some of it\'s features. Just enoght for you to understand
some code without me explaining every bit of it.

> If this chapter seems to \"dirty\" you can check [Xah Lee\'s
> introduction to Emacs
> Lisp](http://xahlee.org/emacs/elisp_basics.html), it will give you a
> better sense of how the language really works.

The checklist
-------------

tralalalala

\[6/7\] ELisp in a few words.
-----------------------------

### [DONE]{.done .DONE} Data types {#data-types}

ELisp comes with your garden variety data types and then some. For now
we\'ll just talk about the basics

Strings
:   need to be quoted with \".

Integers
:   like `-15`, `400` and `666`. Integers are 29 bit of presicion
    instead of your usuarl 32 bit.

Floats
:   , the usual `0.15`, `3.141592` and scientific notation like
    `5.05e-10`.

Characters
:   are prefixed with `?`. So `?a` is the letter `a` , `??` is `?` and
    `?\(` (notice the `\` acting as a escape) is the character `(`.

Booleans
:   , `t` means `true` and `nil` means false. In a boolean context `nil`
    and the empty list `()` are the only falsy values, everything else
    evaluates to `true`.

Symbols
:   Symbols are meant to represent names. A symbol may stand for
    something that we wish to store information about, like `pi`, `nil`
    or `t`. A symbol may also stand for the name itself, in that case we
    need to `quote` the symbol like this \'`a-name`. If a symbol has
    been used to define a variable or function it is said to be *bound*
    otherwise it is *unbound*.

### [DONE]{.done .DONE} Expressions {#expressions}

Like every other Lisp, Elisp is written as nested parenthesized
expressions. The first element in the expression must always be a
function[^1], that\'s why we write

``` {.commonlisp org-language="emacs-lisp"}
(+ 15 600)
```

instead of

``` {.commonlisp org-language="emacs-lisp"}
(15 + 600)
```

Commonly used functions include your usual arithmetic operators
(`+, -, *, <, >,
 <=, >=` and such) plus string, character and list manipulation
functions.

### [DONE]{.done .DONE} Lists {#lists}

As you may (o may not) know Lisp stands for Lis(t) P(rocessing). That
means there are a lot of functions dedicated to manipulating
collections. Nevertheless, I will only talk about lists and their
cousing the association lists because they\'re more than enough for our
purposes.

Let\'s assume for now than anything between parentheses is a list. Since
that\'s exactly the sames syntax used for expressions there must be a
way to differentiate between the two of them. That\'s what quoting is
for. The `quote` function returns it\'s parameters whitout evaluating
any of them so:

``` {.commonlisp org-language="emacs-lisp"}
(quote 1 2 3) ; returns the list (1 2 3)
(quote 1 (+ 1 1) 3); returns the list (1 (+ 1 1) 3)
```

Quoting is so frequently used that there\'s shorthand for it

``` {.commonlisp org-language="emacs-lisp"}
'(1 2 3) ; is the same as (quote 1 2 3)
```

If you need to create a list **and** evaluate its contents you can use
the `list` function like this:

``` {.commonlisp org-language="emacs-lisp"}
(list 1 (+ 1 1) 3); returns (1 2 3)
```

You can add new elements to a list with the `add-to-list` function. It
adds an element to the front of the list **if** it doesn\'t already
exist within it.

``` {.commonlisp org-language="emacs-lisp"}
(setq my-list (list 1 2 3))
(add-to-list 'my-list 4)
```

Association list or *alist* are the other collection type. An alist
looks like a regular list except that every element is a pair in the
form `(itema . itemb)`. Pairs are actually a sort of list themselves so
many list related functions (like `add-to-list`) will also work on them.
You can create alist usinr `quote` like in the following example.

``` {.commonlisp org-language="emacs-lisp"}
; notice the extra set of parentheses
; those are only needed when creating a new
; alist
(setq my-alist '((1.2)))
```

We can now add elements to `my-alist` using `add-to-list`

``` {.commonlisp org-language="emacs-lisp"}
(add-to-list 'my-alist '(3.4))
```

### [DONE]{.done .DONE} Conditions {#conditions}

Elisp has your usual `if/else` construct but some people (me included)
prefer the shorter versions, `when` and `unless`.

``` {.commonlisp org-language="emacs-lisp"}
(if (= 1 1) ; condition
   (message "they're equal") ; then part
   (message "they're not equal")) ; everything else is else-part

(if (= 1 2) ; condition
   (message "they're equal" ; then part
   (message "they're not equal") ; else part
   (message "this will show too"))) ; also else part

(when (= 1 1)
   (message "they are equal"))

(when (not (= 1 2))
   (message "they're not equal"))

(unless (not (= 1 1))
   (message "they are equal"))

(unless  (zerop 1)
   (message "they're not equal"))
```

Each variant will return the value of the last executed expression. In
the case of `if` when the condition fails and there\'s no else part the
return value is `nil`

We also have `cond` which is something like C\'s `switch`

``` {.commonlisp org-language="emacs-lisp"}
(cond
  ;; clauses are in the form ( (test) (body))
  ((= 1 1) (message "they are equal"))
  ((= 1 2) (message "they're not equal"))
  ((string= "equal" "equal") (message "this is string equality")))
```

`cond` tries each clause until one succeeds, that is until it finds one
whose `(test)` part evaluates to `true`. After finding it, it executes
the `(body)` part and returns it\'s value. If no clause succeeds, `cond`
returns `nil`.

### [DONE]{.done .DONE} Loops {#loops}

Elisp has a really nice `while` function which you can use to create
loops, there\'s also a very complex `loop` construct that I will not
cover in this book.

``` {.commonlisp org-language="emacs-lisp"}
;; (while (test) (body))
(setq counter 0)
(while (< counter 10)
(1+ counter))
```

### [DONE]{.done .DONE} Functions and variables. {#functions-and-variables.}

There are two kind of variables in Elisp, global and local. Global
variables are created with `setq` or `defvar`. Since ELisp has no notion
of modules or namespaces they can be accessed everywhere in the
code[^2]. That may seem crazy at first but it provides us with easy
access to internal editor variables like `inhibit-splash-screen` on our
example init file. Local variables are created using `let`, often inside
a functions body.

To create a new function we use `defun` (which ironically is kind of a
function itself)[^3]

``` {.commonlisp org-language="emacs-lisp"}
(defun add (x y)
 "Adds two variables"
(+ x y))
```

We can now call our `add` function like this

``` {.commonlisp org-language="emacs-lisp"}
(add 4 5)
```

There are also anonymous functions (a.k.a **lambdas**), they\'re mostly
used as parameters to high order functions like `map` or to set mode
hooks.

``` {.commonlisp org-language="emacs-lisp"}
(add-hook)
```

Functions may have as many parameters as needed and there are special
syntax for keyword and variable parameters which I would not cover in
these book. A function will always return a value (that of the last
expression), if it has nothing interesting to return then it should
return `nil`

### STARTED External code and features.

Emacs doesn\'t have a module system per se, every file get\'s loaded
into the runtime and it\'s definition either extend or overwrite the
current environment. This process is quite complex but if we get rid of
all the mumbo jumbo it comes down to know this three guys

-   `require` is a function that asks the runtime to install a feature
    if it isn\'t already loaded.
-   `load` also a function, finds a file and loads it\'s content into
    the runtime.
-   `load-path` a variable, a list of the directories where emacs should
    look for files to load.

If feature FEATURE is not loaded, load it from FILENAME. If FEATURE is
not a member of the list \`features\', then the feature is not loaded;
so load the file FILENAME. If FILENAME is omitted, the printname of
FEATURE is used as the file name, and \`load\' will try to load this
name appended with the suffix \`.elc\' or \`.el\', in that order. The
name without appended suffix will not be used. See \`get-load-suffixes\'
for the complete list of suffixes. If the optional third argument
NOERROR is non-nil, then return nil if the file is not found instead of
signaling an error. Normally the return value is FEATURE. The normal
messages at start and end of loading FILENAME are suppressed.

As a convention, every mode or library adds an identifiables symbol to
the global `features` list. Since it\'s a global variable you can query
it\'s contents from your init file to check if some other file has been
loaded an act upon the result. You can check if a feature is enable by
using the function `featurep`.

``` {.commonlisp org-language="emacs-lisp"}
(featurep 'emacs) ; this will always be true
(featurep 'w32) ; this will only be true on Windows (32 bit version of the editor)
```

Evaluating your code in the REPL
--------------------------------

If you want to explore some of the concepts explained in this section
you can try your code in **IELM**. Just type `M-x ielm RET` and it will
open an REPL buffer where you can evaluate ELisp expressions. The IELM
buffer supports all of the editing commands explained before plus `C-up`
(that\'s `Control+ up
arrow`) to go backwards in the prompt\'s history.

Inside an REPL you can call not only commands but regular functions too
so it can be used to interactively change some editor feature. Try
opening a session and write the code below

``` {.commonlisp org-language="emacs-lisp"}
(linum-mode 1) ; this will show line numbers on the left side of the window
```

You\'ve just evaluated a function, which is also the entry point for
LIne NUMber mode. It is a minor mode to display line numbers in the left
margin of the buffer window. Now type `(linum-mode -1)` to change things
back.

Functions, hooks and modes
--------------------------

blah blah blah

Footnotes
=========

[^1]: Not entirely true but please humor me.

[^2]: Again, not the whole truth.

[^3]: Ok, is a macro, but I\'m not talking about macro\'s in this book
