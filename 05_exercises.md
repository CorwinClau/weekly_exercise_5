---
title: 'Weekly Exercises #5'
author: "Xintan Xia"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(ggimage)
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.  

```r
summary <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2021/2021-06-01/summary.csv')

tidy_tuesday_2 <- summary %>%
  ggplot(aes(y = fct_rev(fct_infreq(country)))) +
  geom_bar(aes(text = country)) +
  labs(x = "Number of times chosen",
       y = "",
       title = "Which Ones Are Popular Season Countries?") 

ggplotly(tidy_tuesday_2,
         tooltip = c("text", "x"))
```

<!--html_preserve--><div id="htmlwidget-02720d1a593c49612617" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-02720d1a593c49612617">{"x":{"data":[{"orientation":"v","width":[1,2,2,1,9,1,1,1,1,1,6,2,3,4,1,2,1,1],"base":[9.55,13.55,12.55,8.55,17.55,7.55,6.55,5.55,4.55,3.55,16.55,11.55,14.55,15.55,2.55,10.55,1.55,0.55],"x":[0.5,1,1,0.5,4.5,0.5,0.5,0.5,0.5,0.5,3,1,1.5,2,0.5,1,0.5,0.5],"y":[0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.9,0.9],"text":["count: 1<br />Australia","count: 2<br />Brazil","count: 2<br />Cambodia","count: 1<br />China","count: 9<br />Fiji","count: 1<br />Gabon","count: 1<br />Guatemala","count: 1<br />Islands","count: 1<br />Kenya","count: 1<br />Malaysia","count: 6<br />Nicaragua","count: 2<br />Palau","count: 3<br />Panama","count: 4<br />Philippines","count: 1<br />Polynesia","count: 2<br />Samoa","count: 1<br />Thailand","count: 1<br />Vanuatu"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":75.2511415525114},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Which Ones Are Popular Season Countries?","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.45,9.45],"tickmode":"array","ticktext":["0.0","2.5","5.0","7.5"],"tickvals":[0,2.5,5,7.5],"categoryorder":"array","categoryarray":["0.0","2.5","5.0","7.5"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Number of times chosen","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,18.6],"tickmode":"array","ticktext":["Vanuatu","Thailand","Polynesia","Malaysia","Kenya","Islands","Guatemala","Gabon","China","Australia","Samoa","Palau","Cambodia","Brazil","Panama","Philippines","Nicaragua","Fiji"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18],"categoryorder":"array","categoryarray":["Vanuatu","Thailand","Polynesia","Malaysia","Kenya","Islands","Guatemala","Gabon","China","Australia","Samoa","Palau","Cambodia","Brazil","Panama","Philippines","Nicaragua","Fiji"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"5fb1b81f2bb":{"text":{},"y":{},"type":"bar"}},"cur_data":"5fb1b81f2bb","visdat":{"5fb1b81f2bb":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
Graph1 <- garden_harvest %>%
  filter(vegetable == "tomatoes") %>%
  mutate(week_day = wday(date, label = TRUE), month = month(date, label = TRUE)) %>%
  group_by(month, week_day) %>%
  summarize(sum_weight = sum(weight)) %>%
  ggplot(aes(x = month, y = sum_weight, fill = week_day)) +
  geom_col(position = "fill") +
  labs(x = "",
       y = "",
       fill = "",
       title = "Proportion of each weekday's tomato harvests") +
  theme(panel.grid = element_blank())

ggplotly(Graph1)
```

<!--html_preserve--><div id="htmlwidget-7dd48c13ed0cc4655cbd" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-7dd48c13ed0cc4655cbd">{"x":{"data":[{"orientation":"v","width":[0.9,0.9,0.9,0.9],"base":[0.978905359179019,0.714424345935875,0.879327506017337,0.710314910025707],"x":[1,2,3,4],"y":[0.0210946408209807,0.285575654064125,0.120672493982663,0.289685089974293],"text":["month: Jul<br />sum_weight:   148<br />week_day: Sun","month: Aug<br />sum_weight: 22071<br />week_day: Sun","month: Sep<br />sum_weight:  6668<br />week_day: Sun","month: Oct<br />sum_weight:  5409<br />week_day: Sun"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(68,1,84,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Sun","legendgroup":"Sun","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.9,0.9,0.9],"base":[0.864737742303307,0.667805294619983,0.864686827008343],"x":[1,2,3],"y":[0.114167616875713,0.0466190513158916,0.0146406790089944],"text":["month: Jul<br />sum_weight:   801<br />week_day: Mon","month: Aug<br />sum_weight:  3603<br />week_day: Mon","month: Sep<br />sum_weight:   809<br />week_day: Mon"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(68,58,131,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Mon","legendgroup":"Mon","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.9,0.9,0.9],"base":[0.56057582668187,0.508048029397304,0.726568579546483],"x":[1,2,3],"y":[0.304161915621437,0.159757265222679,0.13811824746186],"text":["month: Jul<br />sum_weight:  2134<br />week_day: Tue","month: Aug<br />sum_weight: 12347<br />week_day: Tue","month: Sep<br />sum_weight:  7632<br />week_day: Tue"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(49,104,142,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Tue","legendgroup":"Tue","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.9,0.9,0.9,0.9],"base":[0.406071835803877,0.336852728825402,0.671987259532729,0.223061268209083],"x":[1,2,3,4],"y":[0.154503990877993,0.171195300571902,0.0545813200137539,0.487253641816624],"text":["month: Jul<br />sum_weight:  1084<br />week_day: Wed","month: Aug<br />sum_weight: 13231<br />week_day: Wed","month: Sep<br />sum_weight:  3016<br />week_day: Wed","month: Oct<br />sum_weight:  9098<br />week_day: Wed"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(33,144,140,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Wed","legendgroup":"Wed","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.9,0.9,0.9],"base":[0.393101482326112,0.238542556219755,0.527788334509655],"x":[1,2,3],"y":[0.0129703534777651,0.0983101726056465,0.144198925023074],"text":["month: Jul<br />sum_weight:    91<br />week_day: Thu","month: Aug<br />sum_weight:  7598<br />week_day: Thu","month: Sep<br />sum_weight:  7968<br />week_day: Thu"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(53,183,121,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Thu","legendgroup":"Thu","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.9,0.9,0.9],"base":[0.084521094640821,0.0890070646688922,0.0777458059612357],"x":[1,2,3],"y":[0.308580387685291,0.149535491550863,0.450042528548419],"text":["month: Jul<br />sum_weight:  2165<br />week_day: Fri","month: Aug<br />sum_weight: 11557<br />week_day: Fri","month: Sep<br />sum_weight: 24868<br />week_day: Fri"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(143,215,68,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Fri","legendgroup":"Fri","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.9,0.9,0.9,0.9],"base":[0,0,0,0],"x":[1,2,3,4],"y":[0.084521094640821,0.0890070646688922,0.0777458059612357,0.223061268209083],"text":["month: Jul<br />sum_weight:   593<br />week_day: Sat","month: Aug<br />sum_weight:  6879<br />week_day: Sat","month: Sep<br />sum_weight:  4296<br />week_day: Sat","month: Oct<br />sum_weight:  4165<br />week_day: Sat"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(253,231,37,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"Sat","legendgroup":"Sat","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":34.337899543379},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Proportion of each weekday's tomato harvests","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,4.6],"tickmode":"array","ticktext":["Jul","Aug","Sep","Oct"],"tickvals":[1,2,3,4],"categoryorder":"array","categoryarray":["Jul","Aug","Sep","Oct"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.05,1.05],"tickmode":"array","ticktext":["0.00","0.25","0.50","0.75","1.00"],"tickvals":[0,0.25,0.5,0.75,1],"categoryorder":"array","categoryarray":["0.00","0.25","0.50","0.75","1.00"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"5fb172af9253":{"x":{},"y":{},"fill":{},"type":"bar"}},"cur_data":"5fb172af9253","visdat":{"5fb172af9253":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains %>%
  filter(avg_delay_all_arriving > -10, avg_delay_all_departing < 30, service == "National") %>%
  ggplot(aes(x = avg_delay_all_departing, y = avg_delay_all_arriving, group = year)) +
  geom_point(size = .5, alpha = .05, color = "darkslategray4") +
  geom_smooth(color = "darkslategray3", se = FALSE, size = 1.5) +
  labs(x = "Average delay for departing trains",
       y = "",
       title = "Average delay for all departing and arriving national trains",
       subtitle = "In minutes. Year: {closest_state}") +
  theme_minimal() +
  theme(panel.grid.minor = element_blank()) +
  transition_states(year,
                    transition_length = 1,
                    state_length = 2.5)
```



![](national_trains_story.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each variety and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
static_plot <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(variety, date) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb)) %>%
  ungroup() %>%
  ggplot(aes(x = date, y = cum_harvest_lb)) +
  geom_area(aes(fill = fct_reorder(variety, cum_harvest_lb, max)))

garden_harvest %>%
  filter(vegetable == "tomatoes") %>%
  group_by(variety, date) %>%
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>%
  ungroup() %>%
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>%
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb)) %>%
  ggplot(aes(x = date, y = cum_harvest_lb)) +
  geom_area(aes(fill = fct_reorder(variety, cum_harvest_lb, max)),
            position = "stack") +
  labs(x = "",
       y = "",
       fill = "",
       title = "Cumulative harvest (lb) for tomato varieties",
       subtitle = "Date: {frame_along}") +
  transition_reveal(date)
```
  


![](tomato_harvest.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.  

```r
bike_image_link <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"

mallorca_map <- get_stamenmap(
    bbox = c(left = 2.28, bottom = 39.41, right = 3.025, top = 39.8), 
    maptype = "terrain",
    zoom = 11
)

mallorca_bike_day7_new <- mallorca_bike_day7 %>% mutate(image = bike_image_link)

ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7_new, 
            aes(x = lon, y = lat, color = ele),
            size = 1.5) +
  scale_color_viridis_c(option ="magma") +
  geom_image(data = mallorca_bike_day7_new,
             aes(x = lon, y = lat, image = image),
             size = 0.08,
             by = "width") +
  scale_size_identity() +
  labs(title = "Mallorca bike riding path",
       subtitle = "Time: {frame_along}") +
  theme_map() +
  theme(legend.background = element_blank()) +
  transition_reveal(time)
```



![](mallorca_bike_path.gif)<!-- -->
  
  I prefer this animation over the static map. This animation can clearly show the path along and the corresponding time, and is more engaging.  
  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_city_map <- get_stamenmap(
    bbox = c(left = -79.58, bottom = 8.91, right = -79.48, top = 9.0), 
    maptype = "terrain",
    zoom = 13
)

panama_races <- bind_rows(panama_bike, panama_run, panama_swim)

ggmap(panama_city_map) +
  geom_path(data = panama_races, 
             aes(x = lon, y = lat, group = event),
             size = 1) +
  geom_point(data = panama_races,
             aes(x = lon, y = lat, color = event),
             size = 3) +
  scale_color_manual(values = c("Bike" = "darkred",
                       "Run" = "darkgreen",
                       "Swim" = "lightblue")) +
  labs(title = "Heather's race path in Panama") +
  theme_map() +
  theme(legend.background = element_blank(),
        legend.position = "bottom",
        legend.title = element_blank()) +
  transition_reveal(time)
```
  

  
![](panama_races.gif)<!-- -->
  
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.  

```r
covid_anim <- covid19 %>%
  group_by(state) %>%
  mutate(seven_days_lag = replace_na(lag(cases, n = 7), 0),
         new_cases_week = cases - seven_days_lag) %>%
  filter(cases >= 20) %>%
  ungroup() %>%
  ggplot(aes(x = log(cases), y = log(new_cases_week))) +
  geom_path(aes(group = state),
            color = "lightgrey",
            alpha = .8) +
  geom_point(aes(color = state), size = 2.5) +
  geom_text(aes(label = state, color = state),                                                   
            check_overlap = TRUE) +
  labs(x = "Log cumulative cases",
       y = "",
       title = "Log Cumulative Cases and Log of New Cases in the Past Week",
       subtitle = "Date: {frame_along}") +
  theme_minimal() +
  theme(legend.position = "none") +
  transition_reveal(date)

animate(covid_anim, duration = 30, nframes = 200)
```



![](covid_anim.gif)<!-- -->
  
  It can be seen that on a large time scale, the log of cumulative cases and log new cases per week are related linearly for most states, despite several states such as Virgin Islands and Northern Mariana Islands, which drastically bounced vertically almost throughout the entire time span, indicating that the condition are not stable at all in these states: The number of new cases fluctuated dramatically.  
  This animation also shows some specific information. For example, New York, the leading state of all states at the beginning, gradually fell behind California, Florida, Texas, and Illinois. Besides that, when around February 2021, all points begin to fall vertically, implying a sharp decrease in the number of new cases in the previous week.
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  



```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")

covid_all_fridays <- covid19 %>%
  mutate(week_day = wday(date, label = TRUE)) %>%
  filter(week_day == "Fri") %>%
  mutate(state = str_to_lower(state)) %>%
  left_join(census_pop_est_2018, by = "state")

covid_density_anim <- covid_all_fridays %>%
  mutate(cases_per_10000 = (cases/est_pop_2018) * 10000) %>%
  ggplot(aes(group = date)) +
  geom_map(map = states_map,
           aes(map_id = state, fill = cases_per_10000)) +
  scale_fill_viridis_c(option = "viridis") +
  expand_limits(x = states_map$long, y = states_map$lat) +
  labs(title = "COVID-19 cumulative case counts in US",
       subtitle = "Per 10,000 residents. Date: {frame_time}",
       fill = "") +
  theme_map() +
  transition_time(date)

animate(covid_density_anim, nframes = 200, end_pause = 10)
```



![](covid_density_anim.gif)<!-- -->
  
  It can be seen that beginning in November, 2020, North and South Dakota started to have a much larger number of cumulative cases per 10,000 residents than other states. Similar things happen for Utah, Tennessee, and Arizona in March, 2021. It's interesting to notice that states mentioned in the previous question, which has a larger number of cumulative case and a faster growth rate in case count per week, are not present here.

## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.  
  The link is [here](https://github.com/CorwinClau/weekly_exercise_5).


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
