The Python chapter
==================

Python is one of my favorite programming languages, it is fun,
cross-platform, simple yet powerful and it comes with a lot of cool
frameworks and utilities (it also has batteries included[^1]).

Emacs and Python have a really good relationship, there are plenty of
modes to help us with writing code and performing tasks like running
code coverage tools, in fact there are so many that it can get a little
confusing sometimes. Is it better to use `virtualenvwrapper.el` or
`pyenv`? Do I use the built-in `python` mode package or install
`python-mode.el` from one of the package archives? As they say in that
old hackers movie: \"There are too many questions\", and to make things
even harder, each one of them has multiple answers.

The checklist.
--------------

Again I\'ll do the checklist thing and then we\'ll go by each item step
by step.

-   How to use Emacs to write Python code.
-   Using virtual Python environments.
-   Navigating throught your code.
-   Interacting with external tools.

How to write Python code
------------------------

Emacs has it\'s very own `python.el` which provides us with the whole
Python editing experience. I\'m talking about syntax highlighting,
automatic indentation, interacting with the shell, code navigation, some
level of code completion, etc. And all we have to do is show up and open
a Python file.

Well, maybe we\'ll have to do a little more than that (I mean, that\'s
what this chapter is all about) but in every step we\'ll be making our
lives easier.

### The Python within

One of the greatest features about `python.el` is running the Python
interpreter in the background (it\'s called *inferior python mode* in
Emacs lingo) and communicate with it by using a regular buffer. Since
this acts as a regular interpreter. It also allow us to do some pretty
nifty things. Here\'s a list of some of the commands:

-   `C-c C-p` Runs the inferior Python interpreter.
-   `C-c C-s` Evaluates the current region in the interpreter.
-   `C-c C-c` Sends the whole buffer for evaluation.
-   `C-c C-l` Evaluates the current **file**, that means everything you
    typed in the buffer and then saved.
-   `C-c C-v` Checks the code in the current buffer for errors.

The name of the interpreter program (which is usually `python`) is
defined in the variable `python-shell-interpreter`. It\'s default value
is just `python` which means Emacs will go search your `PATH` to find
the right program, since the whole thing is based on `commint` we can
start whichever interpreter we like by setting the right value. For
example let\'s change it to run `pypy`

``` {.commonlisp org-language="emacs-lisp"}
; windows users
(setq python-shell-interpreter "c:\\pypy\\bin\\pypy.exe")
```

As you may have noticed in the example, the inferior process doesn\'t
have to be exclusively CPython, we can launch IPython, PyPy and even
IronPython with the same results.

### Syntax and style

Bugs. Yeah, code bugs, the nasty ones, the ones that make your skin
crawl and your test fails. We can\'t do much about them but at least we
can deal with simple syntax errors and in the process make sure people
don\'t go out and get you because you accidentally wrote a 200
characters line of code.

I wan\'t you (to be my virtual env)
-----------------------------------

If you\'re a Python developer then you already know about virtual
environments , either the good old `virtualenv` for Python2 developers
or PEP405\'s `env` module in Python3.

Code completion (with The Force).
---------------------------------

As I said before, code completion saves time and since time is money,
having code completion is actually making you richer[^2]. The default
completion mechanism in `python.el` uses GNU Readline\'s `rlcompleter`
which works fine but sometimes fails to read symbols inside native
modules or guess the types of variables. That\'s why we\'re going to use
`jedi`. But, you may wonder, what\'s `jedi`? David Halter[^3] says that
it is \"an auto-completion tool for Python that can be used for text
editors\". As a bonus it also serves as a code navigation utility.

Setting up `jedi` is really easy, we just need to install the Python
module running `pip` and then install `elpy` and `jedi-core` in the
Emacs package manager menu.

[TODO]{.todo .TODO} Working with frameworks. {#working-with-frameworks.}
--------------------------------------------

There are a lot of web related frameworks in Python

[TODO]{.todo .TODO} Tools of the trade. {#tools-of-the-trade.}
---------------------------------------

aaaaaaaaa

Footnotes
=========

[^1]: Yep, that\'s a Python joke. If you wanna see more then
    `import this`

[^2]: That\'s logic for you.

[^3]: <https://github.com/davidhalter/>
