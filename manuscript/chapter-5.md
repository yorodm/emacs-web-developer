[TODO]{.todo .TODO} Editing text like a pro. {#editing-text-like-a-pro.}
============================================

Writing code involves doing some repetitive tasks like

The checklist
-------------

tralalalal

[TODO]{.todo .TODO} Cut, Copy, Paste and other stuff {#cut-copy-paste-and-other-stuff}
----------------------------------------------------

What is

### [TODO]{.todo .TODO} The Emacs Way {#the-emacs-way}

Aqui va lo del kill ring y etc

### [TODO]{.todo .TODO} The usual way {#the-usual-way}

There\'s a big chance that you\'re working with a GUI environment[^1]
and there\'s an even bigger chance that your environment uses the
standard `C-x`, `C-c`, `C-v` combination to cut, copy, and paste. Well,
Emacs *predates* every modern operating system so it comes with a
different philosophy about how to do those tasks[^2]. Newcommers often
find this discouraging, that\'s why someone decided to include
`cua-mode`.

Common User Access style editing (CUA mode for short) emulates the usual
behavior of GUI editors in Emacs. Once active it will

-   Typing text *replaces* the active selection.
-   Undo, Cut, Copy and Paste work as usual[^3].
-   The *original* Emacs bindings will also work.

Every hard core Emacs user out there will warn you against using CUA and
that\'s for a good reason: `C-c` is the default command prefix sequence
so now if we want to type something like `C-c C-s` it must be done extra
fast!![^4]

``` {.commonlisp org-language="emacs-lisp"}
(cua-mode 1)
```

[TODO]{.todo .TODO} Tabs, spaces and indentation {#tabs-spaces-and-indentation}
------------------------------------------------

Aqui explicas:

-   Indent
-   Auto-fill
-   Lo de las oraciones.
-   Untabify y tabify

Writting less code with snippets
--------------------------------

Having snippets

Footnotes
=========

[^1]: If case you\'re not, don\'t skip the section. It might be of some
    use anyway.

[^2]: Some people may argue that they should\'ve changed the philosophy.
    Those people are wrong.

[^3]: If you\'re in a Unix terminal `C-z` will likely suspend your Emacs
    disregarding the CUA bindings.

[^4]: Actually it musn\'t. You can control the time lapse with
    \<variable que controla que cua reconozca ctr+c como prefijo\>
