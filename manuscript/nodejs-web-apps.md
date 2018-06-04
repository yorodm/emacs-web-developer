The Javascript chapter.
=======================

Javascript y\'all! Making the web development great since the 90\'s!
There are many great editors with Javascript support, even **on line**
editors if you\'re into that, but as usually Emacs brings it\'s own
flavor to the mix.

In this chapter we\'re going to learn how to turn Emacs into a lean (and
mean)[^1] Javascript working environment.

The checklist
-------------

1.  Install a fairly recent version of nodejs (I\'m using `4.4.2`).
2.  Install packages to work with Java/Coffee/Typescript.
3.  Tweak the editor to do some *client side* development.
4.  Do the same for *server side* development.
5.  Have a cup of our favorite hot beverage[^2].

Now go ahead and do the first two items while I start boiling some
water.

Modes for everyone.
-------------------

Like with HTML and CSS, there are several alternatives out there to
write Javascript code

-   Javascript (js2-mode)
-   CoffeeScript (coffee-mode)
-   Typescript (tide)

### js2 or js3?

There are currently two major modes for editing Javascript files,
`js2-mode` and `js3-mode`, for the untrained eye the latter may seem
like the best choice but that\'s only because us, IT people, have a
tendency to jump straight at the biggest version number. Well, our
instincts are wrong this time, `js2-mode` is actually a bit ahead on the
race for language support and plays nicer with others.

### Me likes it\'s coffee.

Back in 2010 Jeremy Ashkenas created a new language to make Javascript
programming even easier, many developers (myself included) jumped right
into the new language and the rest as they say, it\'s history.

### The types of you.

Setting up shop
---------------

Enough nerdsplaining, let\'s do some actual work:

1.  If you haven\'t already, go get [Nodejs](http://nodejs.com) and
    install it.
2.  Install `tern`, `typescript` and `coffeescript`.
3.  Open your Emacs.

Now, depending on your \*script preferences, you can choose to:

1.  Install `js2-mod`, `company-tern`, `js2-refactor` and `xref-js2` for
    working with vanilla Javascript.
2.  Install `coffee-mode`.
3.  Install `tide`, `typescript-mode`

Talk to the browser.
--------------------

tralalala

All about the code.
-------------------

tralalala

Footnotes
=========

[^1]: Pun intended.

[^2]: Get it?
