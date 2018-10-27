---
layout: post
title:  "Intentionally Insulting Exceptions for Python"
date:   2018-10-27 00:00:00 -0400
categories: jekyll update
published: true 
---

I like to leave myself easter eggs when I write code, most often by printing
insults when something goes wrong in the code. Childish? Yes, it but helps
brighten things up when my code is inexplicably broken. This is all well and
good, but I thought I could do a bit better. What if:

+ The insulting messages were real Python `Exception`s rather than lame print
  statements, so that the program fails in a neat and tidy way.
+ The insults were random, so that I don't get bored when the same problem
  happens again and again.

The result of all this is a new package called `insult_error`. You can install
from [PyPI](https://pypi.org/project/insult-error/) or check out the code on
[Github](https://github.com/keithfma/insult_error).

## Automated Insults for Fun and Profit

The `insult_error` package provides a set of insulting exceptions you can use
to make your future self laugh, bother your collaborators, or both.

The core feature is the `InsultError` exception class, which behaves just like
a normal exception with a few differences:
1. The raised error is a randomly-selected subclass with a silly, insulting name
2. If no message is provided, the error will use a random insulting message
3. A special keyword argument `rating` provides some control over how offensive
   you want the error and message to be

A few examples:

```python
from insult_error import InsultError

# raise a random insult with a random message (defaults to "PG" rating)
raise InsultError()
# >>> NotThisAgain: Don't believe everything you think.

# raise a random insult with a user-specified message
raise InsultError('This is my message')
# >>> NotThisAgain: This is my message

# raise a random insult with <= PG rating
raise InsultError(rating="PG")
# >>> ForGodsSake: I don’t have the time or the crayons to explain this to you.

# raise a random insult with <= R rating
raise InsultError(rating="R")
# >>> FuckYouBuddy: I envy people who have never met you.
```

## Implementation Details

There were two tricky bits in implementing this idea: first, how to throw a
random exception type, and second, how to enforce an internal "rating system"
to give users control over how risque they want to be.

To get random exceptions and messages when users raise an `InsultError`, I
(ab)used the `__new__` method. This method gets called first when an object is
instantiated (*before* `__init__` even), and returns an instance of the object.
I used this feature to make `InsultError` a class factory that returns a random
exception instance from the `insult_error` package. This is a wierd thing to do
-- normally a factory method would be a `@classmethod`, but in this case I
wanted to mimic the usage of a normal `Exception` so a method call was out of
the question.

As an aside, the `inspect` module made it relatively easy to automatically
build lists of exception and message options from the package, without having
to write them out manually and maintain them. 

To make ratings work, I added a `rating` attribute to all the exceptions and
methods, and grouped the list of options for exceptions and messages by these
ratings. This means that when the `__new__` method is choosing one of each, I
can respect the users decision about what kind of insults are appropriate. 

## Help Me!

Most importantly, I would *really* welcome contributions to this package.
Random insults are much more fun if I didn't write them all myself!
