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

For the interested, all of the code for this project is posted on Github at:
[github.com/keithfma/parasol](https://github.com/keithfma/parasol).

## TL;DR

Parasol uses high-resolution elevation data to simulate sunshine and constructs
routes that keep users in the sun or shade, whichever they prefer. **You can
try out the app at [parasol.allnans.com](http://parasol.allnans.com).**

**Easier still, check out the demo video below**:

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

The approach is surprisingly straightforward. First, I build a high-resolution
elevation model. Next, I simulate sun/shade using the position of the sun for a
given date and time to illuminate the elevation grid. Then, I compute a custom
cost function that incorporates sun/shade as well as distance for each segment
of the transportatation network. Finally, I apply Djikstra's algorithm to
compute the shortest path given this custom cost. All of this is wrapped up
into a (relatively) user-friendly web application.

To make this a little easier to follow, the figure below shows a high-level
schematic of the inputs and outputs of the system. Later sections provide some
insight into how I designed and implemented each part of the system.

<figure>
<img src="{{page.assets}}/system_overview.png">
<figcaption>
Parasol system overview, showing the input data used to build minimum cost routes that incorporate distance and user sun/shade preference.
</figcaption>
</figure>

The input data I used are:
+ [NOAA LiDAR elevation](https://coast.noaa.gov/dataviewer/)
+ [OpenStreetMap](https://www.openstreetmap.org) via [Overpass API](http://overpass-api.de/)
+ User supplied route endpoints and sun/shade preference

The tools I used are (faaar from exhaustive):
+ Python
+ [Point Data Abstraction Library (PDAL)](https://pdal.io/)
+ [PostgreSQL](https://www.postgresql.org/) with [PostGIS](https://postgis.net/), [pgpointcloud](https://github.com/pgpointcloud/pointcloud), and [pgRouting](https://pgrouting.org/) extentions
+ [GRASS GIS](https://grass.osgeo.org/) ([r.sun](https://grass.osgeo.org/grass75/manuals/r.sun.html) for solar simulation) 
+ [Flask](http://flask.pocoo.org/), [Leaflet](ihttps://leafletjs.com/), [Geoserver](http://geoserver.org/), [Apache](https://httpd.apache.org/) (for the web app) 

## Gridded Elevation from LiDAR

## Simulating Sunshine

## Sun and Shade Cost

## Routing with a Sun/Shade Cost

## Web App

## Next Steps

