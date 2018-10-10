---
layout: post
title:  "Surface Slopes from LiDAR Data (or Fun With Zoning Regulations)"
date:   2018-10-08 10:20:24 -0400
categories: jekyll update
published: true
assets: /assets/2018-10-08-somerville-slope
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

A friend of mine is working with the local zoning board on a rule regulating
"steeply" sloped lots in Somerville MA and asked me to do a quick analysis of
the number of impacted lots. This project turns out to build on some of the
datasets and skills I picked up while building my
[Parasol Navigation app]({{ site.baseurl }}{% post_url 2018-08-07-introducing-parasol %})
during my time as an [Insight Data Science Fellow](https://www.insightdatascience.com/).

The core challenge is a neat one: *how can we compute the slope of the ground in
an urban environment?* The key components to my solution were getting my hands
on high-resolution elevation observations of the ground surface (without
buildings), and figuring out a way to estimate local slopes on a specific
length scale from this noisy data. 

If you want to run or extend this analysis, all of the code and data you need
are hosted at: [github.com/keithfma/somerville-slope](https://github.com/keithfma/somerville-slope).
As always, feel free to contact me if you would like to discuss further.

## Results: Lots of Steep Lots

The zoning rule of interest applies to "all natural slopes exceeding 25% over a
horizontal distance of 30 feet". This sounds like a lot, but percent grade has
a weird definition:  $$\frac{\Delta y}{\Delta x} * 100$$. This means that a
100% grade is a slope of 1, or equivalently of 45 degrees, and that grade can
be more than 100%. The threshold slope for this rule is actually pretty modest:
7.5 feet of elevation change over a horizontal distance of 30 feet.

Not surprising then that in hilly Somerville MA there are a lot of impacted
lots. I find that **2,370 of 13,354 tax parcels (~18%) contain areas at > 25%
grade**. The impacted lots are found in clusters around all of the hilly areas in
the city.

This result should be of interest to the zoning board, as it was initially
assumed that the rule would impact only a few parcels. This is clearly not the
case -- the rule would in fact come into play throughout the city.

<figure>
<img src="{{page.assets}}/parcels.png">
<figcaption>
Map of Somerville MA tax parcels with those impacted by the zoning rule highlighted in red. 
</figcaption>
</figure>

<figure>
<img src="{{page.assets}}/slope.png">
<figcaption>
Surface slopes in Somerville MA as percent grade over a 30 foot length scale,
computed on a 1-meter grid from high-resolution lidar survey data. A few
notable features are 1) high slopes along major roadway and rail lines caused
by retaining walls at thier margins, and 2) high slopes at the edges of parcels
on hilly terrain which are also caused by retaining walls at the edge of the
level lots buildings rest upon. Spot checking in Google Maps Street View show
that the high slopes at parcel edges are not due to buildings mislabeled as
ground observations, as I initially feared.
</figcaption>
</figure>

<figure>
<img src="{{page.assets}}/elevation.png">
<figcaption>
Surface elevations in Somerville MA, computed on a 1-meter grid from
high-resolution lidar survey data.
</figcaption>
</figure>

## LiDAR Elevation from NOAA

Recent LiDAR surveys conducted in the aftermath of superstorm Sandy provide
coverage of the entire city of Somerville. Starting with a shapefiles of MA
towns from MassGIS and of the LiDAR tile footprints from NOAA, I gathered up 13
tiles from the NOAA server. Each tile has an average of 3.8 observations per
square meter. Nice! 

One critical prerequisite for the ground slope analysis to work is that
estimated slopes must exclude buildings and trees. Happily, LiDAR contains some
extra information that helps in labeling and removing these above ground
features (e.g. multiple returns from tree canopy and ground). Even more
happily, this lidar dataset has classifications for all points meaning that I
can simply filter by the label to get the ground-return-only dataset I need.

In total, this ground-only dataset contains ~36 million elevation observations.
There are "holes" wherever buildings were removed from the dataset, but we do
have observations from the ground below the tree canopy.

<!--- TODO: add a figure ploting decimated points -->

## Analysis: 25,781,323 Least Squares Fits

<!-- TODO: add a figure showing one planar fit with the estimated point elevation and gradient direction -->

## Scratch

In this analysis, I used recent (2014) lidar elevation data to compute surface slopes over a 30-ft length scale for Somerville, MA. The lidar dataset provides point observations of elevation with roughly 1 meter horizontal spacing and 20 cm vertical accuracy, and includes classification information that separates point representing the elevation of the ground for those representing natural and man-made objects above the ground (e.g., trees and buildings). The key steps in the analysis are:

1) Gather all bare-ground lidar observations within the city limits.
2) Generate a regular grid with 1 meter horizontal spacing, used to compute gridded elevation and surface slope.
3) For each point in the output grid, fit a 2D plane to all observations within a 15-ft radius using a standard least-squares fit. The gradient of this plane is an estimate of the local surface slope over a 30-ft length scale. Note that grid points with few observations (less than 75% the expected number) are ignored to reduce the impact of any misclassified observations. For example, observations of building edges might be misclassified as ground and lead to erroneously steep slopes. This potential issue is not readily apparent in the results, which suggests the procedure worked as expected.
4) Sum the area with greater than 25% grade in each parcel.
5) Label parcels containing significant are at >25% grade. I set this threshold at 5 m^2, which means a parcel with a few incorrect slope estimates would not be labeled.

The results show 2,370 of 13,354 parcels in Somerville (~18%) contain areas at > 25% grade. This result was initially surprising, but it is reasonable given that a 25% grade is in fact a fairly modest slope: a rise of only 7.5 ft over a distance of 30 ft.

You can find the software I wrote for this analysis at: https://github.com/keithfma/somerville-slope.

## Template

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
