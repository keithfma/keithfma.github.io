---
layout: post
title:  "Parasol Navigation: Optimizing Walking Routes to Keep You in the Sun or Shade"
date:   2018-08-02 10:20:24 -0400
categories: jekyll update
published: true

assets: /assets/2018-08-02-introducing-parasol
---

Sun and shade has a strong impact on how many of us pick a route to the places
we want to go. For me personally, direct sun is the devil himself. The point of
[Parasol](http://parasol.allnans.com) is to leverage knowledge of the
enviroment to help people make these decisions. This is analogous to how we use
navigation apps like Google Maps or Waze to navigate around traffic: we want to
avoid congested routes and the apps use extra information about current
conditions to help us do so.

I built *Parasol* over 4 weeks as an independent project with [Insight Data Science](https://www.insightdatascience.com).
I cannot recommend this fellowship highly enough. If you are a PhD considering
a career in data science, [apply to Insight](https://www.insightdatascience.com/apply).
If you are a company looking for newly minted data scientists, [become a partner](https://www.insightdatascience.com/partnerships).

## TL;DR

Parasol uses high-resolution elevation data to simulate sunshine and constructs
routes that keep users in the sun or shade, whichever they prefer. **You can
try out the app at [parasol.allnans.com](http://parasol.allnans.com). Easier
still, check out the demo video below**:

<figure>
<video controls src="{{page.assets}}/ParasolDemo.mp4" type="video/mp4" width="700" height="525">Sorry, your browser doesn't support embedded videos</video>
<figcaption>
The demo starts by finding the shortest-distance path between the Esplanade and
South Station in Boston. Next, the sun/shade layer is turned on, with light
colors indicating sun and dark colors indicating shade. Then, I use the
sun/shade preference slider to update the route to prefer shade, and show that
the resulting route is 19% longer with 13% less sun. Shifting the slider to
perfer sun yields a different route that is 5% longer than the shortest path
with 18% more sun. Finally, I change the time-of-day and show a different best
sunny route that is 2% longer and has 25% more sun.
</figcaption>
</figure>

## Approach and System Overview

## Gridded Elevation from LiDAR

## Simulating Sunshine

## Sun and Shade Cost

## Routing with a Sun/Shade Cost

## Web App

## Next Steps

