---
layout: post
title:  "Always Insult Me: an insult-error update"
date:   2020-04-18 07:00:00 -0400
categories: jekyll update
published: True 
---

You may recall the amazing [insult-error](https://pypi.org/project/insult-error/) python package
from a ["recent" post]({% post_url 2018-10-27-insult-error %} ). Well, great news! It is now slightly
better in version 0.3.1.

This update brings a new feature to make it super easy to start insulting friends and enemies alike. 
Running the `always_insult_me` function will rejigger your environment to convert all uncaught exceptions
into `InsultError`s with the usual random insults and (optionally) messages. If you get sick of this
behavior, `dont_always_insult_me` will set things back to normal. 

A little example to demonstrate:

{% highlight python %}
from insult_error import always_insult_me, dont_always_insult_me

# turn on ubiquitous insults
# uncaught exceptions converted to InsultErrors and messages replaced too
always_insult_me()
raise ValueError('a normal message')
# >>>
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# FuckYouBuddy: I don't have the time or the crayons to explain this to you

# turn off insults, things go back to normal
dont_always_insult_me()  
raise ValueError('another normal message')
# >>>
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# ValueError: another normal message

# turn insults back on, but this time preserve error messages
always_insult_me(preserve_msg=True)
raise ValueError('a normal message')
# >>>
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# FuckYouBuddy: a normal message
{% endhighlight %}

Credit to Jacob MacDonald (my colleague at Indigo Ag) for the idea and initial implementation. This feature
works by modifying the `sys.excepthook` callable, which is executed just before Python fails due to an 
uncaught exception. Happily, the original value of this callable is preserved as `sys.__excepthook__`, so
we can easily undo the change.We simply wrap the original hook such that the exception type becomes an `InsultError`, 
but the traceback is unchanged. This is important because **we want to insult people, but not fuck up
their workflow**. 

Some internals have changed as well since the last post. Most notably, error names are handled in
a much simpler way. Rather than mucking about with `__new__` and exception subclasses, we just
modify the exception `___class__.__name__` attribute during exception initialization. We still
have an `InsultError` instance, but it has a nice random name. Python really let's you do any
damn thing you want. 

Lastly, there are a few new insults in there since my last post. Still looking for contributions, of course!
