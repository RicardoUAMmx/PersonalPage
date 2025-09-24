---
title: Capturing stringify strings
date: 2025-09-22 05:00:00 +0800
categories: [Programming, Compilers]
tags: [c, dfa, strings]
description: Compiler principles. Capturing strings.
pin: true
---

# Capturing stringify strings

Sometimes programmers tend to be too tidy or at least its a need to perform
its job, because if we think about it, programming its just a traceback of
given reason leaved behind. The name of the game: Logic.

![Strings Flow Folded](/assets/img/strings-flow-condensed.png){: width="700" height="400" .normal }
_String Flow Chart_

The first thing that we should do to capture stringify strings is know its
definition. In basic terms, a **string** in that sense, is everything wraped by
**double quotes**. If we would like to write a string in that way, first we
write our first quote, then everything you wish, and then close it with the
last double quotes. It is characterized by a **string flow chart**.

![Strings Flow Folded](/assets/img/strings-flow-expanded.png){: width="700" height="400" .normal }
_String Flow Chart_

At this point we have:

- Definitions: Based on the working alphabet, that could be english characters
and symbols, adding special slashed characters, a slash followed by an element
of a selected characters, and if that element would be the "u" character must
be followed by 4 digits.

- A flow chart: It represents the way to capture a string while the writing
action happen. This is very important because we could know at every moment if
we are able to write certain character and if it is correct.

So, we know what to write and how to write it and determine if it is rightly
wroten. For example: We know or possibilities once we have written a **double
quote**:

1. A writable english character or symbol.
2. A slash followed by a one character code, and if that code is and "u", must
ne followed by 4 hexadecimal digits.
3. Or close the string with another double quote.

And with the above we know if it is right, because we dont have any string if
we havent close it.
