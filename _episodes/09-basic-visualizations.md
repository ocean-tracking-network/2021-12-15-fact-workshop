---
title: Basic Visualization and Plotting
teaching: 30
exercises: 0
questions:
    - "How can I use glatos to plot my data?"
    - "What kinds of plots can I make with my data?"
---

## FACT Node

Now that we've cleaned and processed our data, we can use `glatos`' built-in plotting tools to make quick and effective visualizations out of it. One of the simplest visualizations is an abacus plot to display animal detections against the appropriate stations. To this end, `glatos` supplies a built-in, customizable `abacus_plot` function.

~~~
# Visualizing Data - Abacus Plots ####
# ?glatos::abacus_plot
# customizable version of the standard VUE-derived abacus plots


abacus_plot(detections_w_events,
            location_col='station',
            main='TQCS Detections by Station') # can use plot() variables here, they get passed thru to plot()


~~~
{: .language-r}

This is good, but you can see that the plot is cluttered. Rather than plotting our entire dataset, let's try filtering out a single animal ID and only plotting that. We can do this right in our call to `abacus_plot` with the filtering syntax we've previously covered.

~~~
# pick a single fish to plot
abacus_plot(detections_filtered[detections_filtered$animal_id=="TQCS-1049273-2008-02-28",],
            location_col='station',
            main="TQCS-1049273-2008-02-28 Detections By Station")
~~~
{: .language-r}

Other plots are available in `glatos` and can show different facets of our data. If we want to see the physical distribution of our stations, for example, a bubble plot will serve us better.

We'll continue to use the Florida raster `FL` from last lesson. Remember that we need to provide a coastline shape to plot our data on top of.

~~~
# Bubble Plots for Spatial Distribution of Fish ####
# bubble variable gets the summary data that was created to make the plot
detections_filtered

?detection_bubble_plot

bubble_station <- detection_bubble_plot(detections_filtered,
                                out_file = '../tqcs_bubble.png',
                                location_col = 'station',
                                map = FL,
                                col_grad=c('white', 'green'),
                                background_xlim = c(-81, -80),
                                background_ylim = c(26, 28))
bubble_station

bubble_array <- detection_bubble_plot(detections_filtered,
                                      background_xlim = c(-81, -80),
                                      background_ylim = c(26, 28),
                                      map = FL,
                                      out_file = 'act_bubbles_by_array.png')
bubble_array
~~~
{: .language-r}

These examples provide just a brief introduction to some of the plotting available in `glatos`.

> ## Glatos FACT Challenge
>
> Challenge 1 ----
> Create a bubble plot of that bay we zoomed in earlier. Set the bounding box using the provided nw + se cordinates, change the colour scale and
> resize the points to be smaller. As a bonus, add points for the other receivers that don't have any detections.
> Hint: ?detection_bubble_plot will help a lot
> Here's some code to get you started
> ~~~
> nw <- c(-81, -80) # given
> se <- c(26, 28) # given
> ~~~
> {: .language-r}
>
> > ## Solution
> >
> > ~~~
> > nw <- c(38.75, -76.75) # given
> > se <- c(39, -76.25) # given
> >
> > bubble_challenge <- detection_bubble_plot(detections_filtered,
> >                                      background_ylim = c(nw[1], se[1]),
> >                                      background_xlim = c(nw[2], se[2]),
> >                                      map = FL,
> >                                      symbol_radius = 0.75,
> >                                      location_col = 'station',
> >                                      col_grad = c('white', 'green'),
> >                                      receiver_locs = receivers, # For bonus
> >                                      out_file = 'fact_bubbles_challenge.png')
> > ~~~
> > {: .language-r}
> {: .solution}
{: .challenge}
