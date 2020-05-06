+++
showonlyimage = false
draft = false
image = "img/portfolio/spacerCompressed.jpg"
date = "2020-04-05"
title = "spacer śladami Szarych Szeregów"
weight = 1
+++

historical geo game in Warsaw
<!--more-->

![logo](/img/portfolio/spacerCompressed.jpg)

# Reliving the history

*This game took place in Warsaw during the March of 2020.* 

*Avalilable only in Polish.*

[**direct link**](http://spacer.mokotow.zhp.pl/)

Your task was to visit the important places in the history of scouting before the Warsaw Uprising in 1944 in real life, with guided information about each place. Think of it as a themed tour.

There were three difrent trails:
* Zawiszacy - _2 hours to complete_
* Bojowe Szkoły - _2.5 hours to complete_
* Grupy Szturmowe - _3 hours to complete_
  
Each trail was color coded.

In total there were **24 different points**.

![plate example](/img/portfolio/spacer-plate.jpg)
*Example game point (plate in bottom left)*

Each level required difrent amount of time and skills to complete.

## My part

I was responsible for the technical side of the event. I made the website that interacts with the user and lets him use codes that are displayed on the plates to show the content of the place in game.
This way, the player doesn't have to download any app, the whole experience takes part in the browser.


The process was to design the network of points that would link from one to another, u gradually telling the story to the audience.

```
Start -> Point 1 -> Point 2 -> Point... -> Finish
```

If the user walks up to a point, they can input the text they see on a plate to the website. After that, they can read about the history of this curretnt place, and people that were connected to it. After that they can press a button to open directions in Google Maps to the next point.

The whole website runs on [Jekyll](https://jekyllrb.com/), and doesn't use HTTPS out of scouting limitations.

![example plate](/img/portfolio/examplePlate.png)
*plate design*


![example plate](/img/portfolio/miejsce-Na-Kod.png)
*code checker*


![example plate](/img/portfolio/spacer-content.png)
*example content on a point*
