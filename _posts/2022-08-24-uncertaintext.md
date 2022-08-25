---
layout: post
title:  "Uncertaintext: an experiment in representing uncertainty with text"
date:   2022-08-24 00:00:00 -0400
assets: /assets/2022-08-24-uncertaintext
published: true 
---

<script type=module>
  import uncertaintext from "{{page.assets}}/uncertaintext.js";
  uncertaintext();
</script>

<style>
  .uncertaintext {
    font-family: monospace;
  }
</style>

In scientific writing, the typical ways to report uncertainty are with the
friendly plus-or-minus sign, e.g., 1.5 ± 0.05, or the more concise but more
cryptic parenthesis notation, e.g., 1.5(5).

These representations are understood to give a range within which the true
value is likely to fall -- the textual equivalent of error bars in a plot.
While it can be tricky to pin down the precise meaning of the stated
uncertainty (a 95% confidence interval? the standard error? a Bayesian credible
interval?), authors can spill a little extra ink to indicate what kind of range
they have shown and the notation works beautifully.

In popular writing, it seems more common to avoid any technical notation (e.g.,
±). Instead, you are likely to see either a range of values (e.g. "between 1 and 3")
or just some fuzzy modifier (e.g., "about 2"). This representation trades detail
about the uncertainty for approachability, which makes perfect sense for a 
more informal target audience.

Where I think both approaches come up short is in conveying an _intuition_ of
what values are likely. Sure, the information is there, but we know that
probability is hard and uncertainty often misunderstood.

My [uncertaintext project][uncertaintext-repo] is a little experiment aimed at delivering this
intuition. Let's take advantage of the fact the webpages need not be static,
and see what happens if we display uncertain values as samples from some
underlying distribution. 

## Uncertaintext in action

Take this example sentence about the current estimate of sea level rise provided by [NASA][nasa-sea-level]:

> Satellite sea level observations show 101 ± 4.0 mm rise in global sea level since 1993.

Let's jazz it up a bit using the uncertaintext library. Taking a guess that the
measurement uncertainty is Gaussian, and that the stated 4.0 range is two
standard deviations, we can use special HTML markup to indicate that we want to
display a normal distributes with mean 101 and standard deviation 2.
Uncertaintext will find and parse this information when the page loads, and
insert a sample from the specified distribution into the text.  This value is
updated (replaced) several times a second. Observe the fun!

<blockquote>
  Satellite sea level observations show 
  <span class=uncertaintext data-uct-distrib=normal data-uct-mu=101 data-uct-sigma=2 data-uct-format="&nbsp;>6.2f"></span> mm 
  rise in global sea level since 1993.
</blockquote>

Maybe that's too much precision? We can update the format to give fewer digits.

<blockquote>
  Satellite sea level observations show 
  <span class=uncertaintext data-uct-distrib=normal data-uct-mu=101 data-uct-sigma=2 data-uct-format="&nbsp;>3d"></span> mm 
  rise in global sea level since 1993.
</blockquote>

Maybe that update cadence is too fast and giving you a headache? We can specify
the update in "frames-per-second" to slow things down.

<blockquote>
  Satellite sea level observations show 
  <span class=uncertaintext data-uct-distrib=normal data-uct-mu=101 data-uct-sigma=2 data-uct-format="&nbsp;>3d" data-uct-fps=1></span> mm 
  rise in global sea level since 1993.
</blockquote>

Maybe you want a different distribution? We can try a uniform distribution
instead, here with a minimum of 97 and a maxium of 105.

<blockquote>
  Satellite sea level observations show 
  <span class=uncertaintext data-uct-distrib=uniform data-uct-min=97 data-uct-max=105 data-uct-format="&nbsp;>3d" data-uct-fps=1></span> mm 
  rise in global sea level since 1993.
</blockquote>


Personally, I find this dynamic representation informative if a bit distracting.

TODO: discuss tradeoff between communicating precise info and conveying an
intuition for the value and how well we have pinned it down.

## How it works and how to use it

TODO

[uncertaintext-repo]: https://github.com/keithfma/uncertaintext
[nasa-sea-level]: https://climate.nasa.gov/vital-signs/sea-level/
