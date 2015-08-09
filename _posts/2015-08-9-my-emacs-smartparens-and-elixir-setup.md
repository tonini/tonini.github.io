---
layout: post
title:  "My Smartparens and Elixir setup"
date:   2015-08-09
categories: elixir emacs smartparens
---

As you all could imagine I write a great amount of Emacs Lisp because for [Alchemist][alchemist].
To get a bit of support while writing all these parentheses I use a minor mode called [Smartparens][smartparens].

It quotes itself as the following:

>  ... deals with parens pairs and tries to be smart about it.

For someone who writes Emacs Lisp regularly the [Smartparens][smartparens] also offers great functionality to move between expressions for example, which are really handy.

But I guess for you Elixir guys this isn't what you looking for. So what I like about [Smartparens][smartparens] too is the ability to add new custom pairs, like in our situation as Elixir programmers `do`/`end` for example.

There are two situations where I like the auto pairing, in `do`/`end` or `->`/`end` cases.

Let's see how my custom setup looks like to realize the desired behavior:

{% highlight emacs %}
(defun my-elixir-do-end-close-action (id action context)
  (when (eq action 'insert)
    (newline-and-indent)
    (forward-line -1)
    (indent-according-to-mode)))

(sp-with-modes '(elixir-mode)
  (sp-local-pair "->" "end"
                 :when '(("RET"))
                 :post-handlers '(:add my-elixir-do-end-close-action)
                 :actions '(insert)))

(sp-with-modes '(elixir-mode)
  (sp-local-pair "do" "end"
                 :when '(("SPC" "RET"))
                 :post-handlers '(:add my-elixir-do-end-close-action)
                 :actions '(insert)))
{% endhighlight %}

First, we add the new pairs just for the `elixir-mode` major mode. Secondly we define our desired start and end pairs, in our case we start with `"->"` / `"end"`.
The next thing we need to inform [Smartparens][smartparens] about is the keys which should trigger the closing action. For the first pair definition, these would be just `"RET"` (Return). For a correct indentation after our favored pairing is done, we add our custom function `my-elixir-do-end-close-action` to the post handlers. These functions will be called by [Smartparens][smartparens] after the pairing is done, in our case we just obtain our desired indentation.

That's all, you now will have these two auto pairings available inside your Elixir files.

I'm not sure about any other pairings which would be useful, if you have some ideas I would be glad to hear it.

[smartparens]:    https://github.com/Fuco1/smartparens
[alchemist]:      https://github.com/tonini/alchemist.el
