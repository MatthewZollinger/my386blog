---
layout: post
title:  "EFY Album Data Part 2: Exploratory Data Analysis"
author: Matthew Zollinger
description: An EFY EDA!
image: /assets/images/efy2007.jpg
---

Well, here we are. We already scraped SingPraises.net and individual YouTube videos for track names, credits, and writers in [my first post on EFY song data](https://matthewzollinger.github.io/my386blog/2023/03/17/efy-songs-a.html). In [my second post](https://matthewzollinger.github.io/my386blog/2023/04/04/efy-songs-b.html), we looked at various graphs of song length, number of artist contributions, and repeat songs.

But now it's time for the main event: *which EFY song writer writes the longest songs*? As a reminder, because SingPraises.net only links YouTube videos that are verified as legally uploaded, we only have data for songs released from 2009 onward. Additionally, I figured that writing the music was probably a bigger contributor to song length than lyric writing, so we'll once again use music writers with 3 or more credits to see who writes the longest songs.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/songlengths.png" alt = "Graph of all-time most credited artists for EFY albums." style="width:500px;"/>

Tyler Castleton edges out the other composers from having the highest median song length; he also has the highest third-quartile song length and the second highest first-quartile song length after Hilary Weeks. His sheer volume of output likely meant he wrote all sorts of songs, short and long, for EFY. Examing mean song length, however, reveals that Hilary Weeks's outlier (the longest song in the data set) actually brings her mean song length higher than Castleton's (she has fewer songs than any of the other artists here). Of the op contributing artists, Stephanie Smith Mabey appears to have the lowest, with lower quartiles, minimums, and maximums besides Nik Day's outlier minimum.

(Also, if you read my last post, you may notice we're missing a couple artists from last time. I realized I coded my filters wrong and included songs from 2008 onward, rather than 2009 onward, in those graphs. I plan to go back and update those graphs later, but for now, know that Staci Peters only had two songs in the 2009-2019 range and Barry Gibbons had none, hence their exclusion from this chart.)

Thanks for following me on this journey! Maybe soon I'll add more track length data - YouTube isn't the only place I can get it, after all. Feel free to check out [the GitHub repo for this project](https://github.com/MatthewZollinger/EFY-Album-Data) if you'd like to play around with the data yourself or mess with my code. Let me know what other analysis of this data you'd like to see in the comments!