---
layout: post
title:  "Uncertaintext: text-only animation of uncertainty"
date:   2023-06-04 00:00:00 -0400
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
they have shown and the notation works just fine.

In popular writing, it seems more common to avoid any technical notation (e.g.,
±). Instead, you are likely to see either a range of values (e.g. "between 1 and 3")
or just some fuzzy modifier (e.g., "about 2"). This approach trades detail
about the uncertainty for approachability, which makes perfect sense for a 
more informal target audience.

Where I think both approaches come up short is in conveying an _intuition_ of
what values are likely. Sure, the information is there, but we know that
probability is hard and uncertainty often misunderstood.

My [uncertaintext project][uncertaintext-repo] is a little experiment aimed at
delivering this intuition. Let's take advantage of the fact the webpages need
not be static, and see what happens if we display uncertain values as a simple
text-only animation of samples from some underlying distribution. 

## Uncertaintext in action

Take this example sentence about the current estimate of sea level rise
provided by [NASA][nasa-sea-level]:

> Satellite sea level observations show 101 ± 4.0 mm rise in global sea level since 1993.

Let's jazz it up a bit using the [uncertaintext][uncertaintext-repo] library.
Taking a guess that the measurement uncertainty is Gaussian, and that the
stated 4.0 mm range is two standard deviations, we can tell uncertaintext to
display samples from a normal distributes with mean 101 and standard deviation
2. Observe the fun!

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

Personally, I find this dynamic representation informative, if a bit silly. I
think that it _does_ impart a more visceral feel for the certainty of the
estimated value. 

As a final example, say we have two estimates with the same expected value but
different standard deviations. Displaying these with uncertaintext hammers home
the difference in a quite satisfying way:

<table>
  <tr>
    <th> Standard Notation </th>
    <td> 1 ± 0.01 </td> 
    <td> 1 ± 0.5 </td> 
  </tr>
  <tr>
    <th> Uncertaintext </th>
    <td><span class=uncertaintext data-uct-distrib=normal data-uct-mu=1 data-uct-sigma="0.01" data-uct-format="&nbsp;>.2f" data-uct-fps=2></span></td>
    <td><span class=uncertaintext data-uct-distrib=normal data-uct-mu=1 data-uct-sigma="0.5" data-uct-format="&nbsp;>.2f" data-uct-fps=2></span></td>
  </tr>
</table>

## How to use it

Uncertaintext is a javascript module which finds HTML elements with
`class=uncertaintext`, parses special dataset attributes that define the
distribution, format, and update interval, and then inserts random samples into
the element's `innerHTML`.

First, download uncertaintext.js [LINK TO DISTRIBUTION NEEDED] and put it
somewhere accessable to your webpage. 

Then, import the main uncertaintext function into your page and run it, like so:
```html
<script type=module>
  import uncertaintext from "include/uncertaintext.js";
  uncertaintext();
</script>
```

I've found that using a monospace font is essential too. Otherwise, the
changing character widths nudge the surrounding text and make the page layout
unstable. A little bit of styling does the trick:
```html
<style>
  .uncertaintext {
    font-family: monospace;
  }
</style>
```

Lastly, add `<span>` elements where you want uncertaintext to display random
samples from a given distribution. For example:
`<span class=uncertaintext data-uct-distrib=normal data-uct-mu=1 data-uct-sigma="0.01"></span>`

This is, in fact, exactly how this blog post uses uncertaintext. For more
details on available distributions and other formatting options, take a look at
the [README for the project][uncertaintext-repo].

[uncertaintext-repo]: https://github.com/keithfma/uncertaintext
[nasa-sea-level]: https://climate.nasa.gov/vital-signs/sea-level/
