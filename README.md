Ever wonder what Bill Murray might look like if he were the same colors as a bowl of celery?

Perhaps you've been curious about how you might convert the colors of your ggplots into those of your favorite desert vista...

What if I told you these daily quandaries had been solved?

**Search no more, for here lay your answers! Read on to discover the answers to these questions, and more!**

RImagePalette
=============

Extract colors from an image, then use them in plots or for fun!
----------------------------------------------------------------

The `RImagePalette` package is a pure R implementation of the median cut algorithm. This package lets you use the colors from an image you like to create pretty plots, or to swap colors from one image to another.

Note: There is an element of randomness in the median cut algorithm, so set your seeds carefully, and try running the algorithm a few times if you aren't happy with the scale.

This package is available from [CRAN](https://cran.r-project.org/web/packages/RImagePalette/index.html) using:

``` r
install.packages("RImagePalette")
library(RImagePalette)
```

Images as Scales
================

Here's a common situation: you've just made a plot using your favorite plotting package, `ggplot2`, but just can't find the right colors to represent the points. While contemplating your dilemma, your mind drifts off to this wonderful photo of the desert you took last week. What should you do?

Why not combine the two thoughts? The `RImagePalette` package let's you do just that:

First, load your image:

``` r
desert <- jpeg::readJPEG("figs/Desert.jpg")
display_image(desert)
```

![](figs/README/desert-1.png)

Then plot it using the new `scale_color_image`:

``` r
#Create your plot, and use scale_color_image:
library(ggplot2)
p <- ggplot(data = iris, aes(x=Sepal.Length, y=Sepal.Width, col=Species)) + geom_point(size=3)
p + theme_bw() + scale_color_image(image=desert)
```

![](figs/README/desertplot-1.png)

Images as Continuous Scales
===========================

Often times, discrete scales aren't what you need. Using `discrete = FALSE` in the scale, we can use linear interpolation to create a reasonable scale.

``` r
p <- ggplot(data = iris, aes(x=Sepal.Length, y=Sepal.Width, col=Petal.Length)) + geom_point(size=3)
p + theme_bw() + scale_color_image(image=desert, discrete=FALSE, n=3) # 'n' is the number of colors extracted from the image
```

![](figs/README/desertplotcontinuous-1.png)

Viewing Palettes
================

It is also simple to create palettes from an image using the `create_palette()` function. Let's use the `show_col` function from `scales` to take a look:

``` r
library(scales)
lifeAquatic <- jpeg::readJPEG("figs/LifeAquatic.jpg")
display_image(lifeAquatic)
```

![](figs/README/lifeaquaticscale-1.png)

``` r
scales::show_col(create_palette(lifeAquatic, n=9))
```

![](figs/README/lifeaquaticscale-2.png)

we can play with the settings to tweak the scale to our liking:

``` r
lifeAquaticPalette <- create_palette(lifeAquatic, n=9, choice=median, volume=TRUE)
scales::show_col(lifeAquaticPalette)
```

![](figs/README/lifeaquaticscale2-1.png)

If it contains colors we like, we can pick and choose, and use them in a manual scale:

``` r
p <- ggplot(data = iris, aes(x=Species, y=Sepal.Width, fill=Species)) + geom_bar(stat="identity")
p + theme_bw() + scale_fill_manual(values=lifeAquaticPalette[c(2,3,6)])
```

![](figs/README/lifeaquaticbar-1.png)

Just for fun
============

You didn't think I forgot about old Bill and celery plate complexion, did you?

We can swap colors across images using the `switch_colors()` function:

``` r
celery <- jpeg::readJPEG("figs/CeleryLunch.jpg")
billMurray <- jpeg::readJPEG("figs/BillMurray.jpg")
```

![](figs/README/celandbill-1.png)

``` r
switch_colors(billMurray, celery, source_colors = 5)
```

![](figs/README/celerybill-1.png)

Let's see what Barack Obama might look like on the 4th of July this year:

``` r
america <- jpeg::readJPEG("figs/AmericanFlag.jpg")
obama <- jpeg::readJPEG("figs/Obama.jpg")
switch_colors(obama, america, source_colors=5)
```

![](figs/README/obamerica-1.png)

Inspiration:
============

<https://github.com/lokesh/color-thief/blob/master/src/color-thief.js>

<https://github.com/karthik/wesanderson>

<http://www.r-bloggers.com/towards-yet-another-r-colour-palette-generator-step-one-quentin-tarantino/>
