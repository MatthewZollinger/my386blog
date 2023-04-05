---
layout: post
title:  "EFY Album Data Part 2: Exploratory Data Analysis"
author: Matthew Zollinger
description: An EFY EDA!
image: /assets/images/efy2007.jpg
---

Picking up where I left off in [my last post](https://matthewzollinger.github.io/my386blog/2023/03/17/efy-songs-a.html), it's time for some exploration of EFY song data. As a reminder, this data contains album titles, song titles, credits (artists, words, music, and arrangment/tranlsation) for each song, YouTube links, and song durations. Because [SingPraises.net](singpraises.net) strives for integrity, only YouTube videos that could be ascertained to be legal uploads have YouTube links (and, subsequently, lengths) - this happens to exclude all albums before 2009.

That being said, I still wanted to take a look at the data from the earliest days of EFY, because I'm a nerd about these things.

Because of the way I wrote the code, there are, for example, 11 columns for artists (as one song has 11 credited artists!). Obviously this makes it a bit hard to analyze credited individuals, so I had to do a bit of data wrangling to create these plots.

## Credits (All Time)

For graphical clarity, for each credit type I've dropped any musicians with fewer than 6 of that credit.

### Artists

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/artists.png" alt = "Graph of all-time most credited artists for EFY albums." style="width:500px;"/>

Kenneth Cope is one of the most prolific singer-songwriters in the Latter-day Saint music scene; it's little surprise that he holds the position of most contributing EFY artist (24 songs). I had rarely heard of Julie de Azevedo, however, who claims second (23 songs). The slightly later-debuting Jenny Jordan Frogley takes third (19 songs), with Michael Webb and Greg Simpson in fourth and fifth (15 and 13 songs respectively).

### Lyric Writers

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/words.png" alt = "Graph of all-time most credited word writers for EFY albums." style="width:500px;"/>

Tyler Castleton wrote or contributed to a stunning 72 songs across all of EFY. There's a steep dropoff for the next four slots, with Staci Peters, Russ Dixon, Nik Day, and Kenneth Cope taking these spots (35, 34, 34, and 33 songs respectively).

### Music Writers

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/music.png" alt = "Graph of all-time most credited music writers for EFY albums." style="width:500px;"/>

Tyler Castleton takes top spot again, with 74 songs this time. Staci Peters is once again second with 36, followed by a three-way tie between Russ Dixon, Nik Day, and Kenneth Cope at 34 songs. And here I thought that Kenneth Cope and Nik Day were prolific...

As a side note, you'll notice that the lyric and music graphs are quite similar. After a somewhat tedious subsetting operation, I discovered that, of the 388 songs in our data set, only 40 (10%) do not have any music writers who also wrote the lyrics. Which is totally fair, since music and lyrics usually need some cohesion.

## Credits (2008-2019)

Since this is the data set I'll primarily be working with, I decided to look at it separate from the older songs. This time exclusions were made dependent on the credit, as described in each credit's section.

### Artists

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/artists08.png" alt = "Graph of most credited artists for EFY albums from 2008 to 2019." style="width:500px;"/>

(This graph excludes artists with fewer than 3 artist credits.)

The age of Kenneth Cope appears to have ended by this point (he had one song during this era, "Treasure the Truth" from the 2010 album), but it appears he's been supplanted by Nik Day, who leads with 7 songs. Scott Krippayne comes in second with 6, and Patch Crowe, Stephanie Smith Mabey, and Ryan Innes each have 5.

Nik Day's 7 songs looks impressive in the modern era, but keep in mind there are 12 albums here, for a singing ratio of about 0.6 songs per album. Kenneth Cope contributed 23 songs on 33 albums total (discounting the 1986 album, which was one song by Cope, for both counts) for an average of about 0.7 songs per album.

### Lyric Writers

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/words08.png" alt = "Graph of most credited word writers for EFY albums from 2008 to 2019." style="width:500px;"/>

(This graph excludes writers with fewer than 2 lyric credits.)

This era is a strong subset for Tyler Castleton, who wrote or contributed 37 songs. Here we see Nik Day and Russ Dixon in their heyday; all of their lyric credits come from this era (34 songs each). Scott Krippayne has an even stronger presence here than in singing, with 22 songs to his name; Stephanie Smith Mabey is just behind at 21 songs.

### Music Writers

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/music08.png" alt = "Graph of most credited music writers for EFY albums from 2008 to 2019." style="width:500px;"/>

(This graph excludes writers with fewer than 3 music credits.)

Castleton has 38 contributions here, with Russ Dixon and Nik Day at 34 and Scott Krippayne and Stephanie Smith Mabey maintaining their 22 and 21 respective songs.

Considering the lyric and music crossover again, 10 of our 40 songs (25%) with no overlap in these credits came from this era; 136 the 388 songs overall (35%) came from this era. 10 to 136 (7%) is quite a bit lower than the overall percentage - perhaps collaborating this way is becoming less common?

## Length

Next, I'll consider lengths of songs - first as a whole, then grouped by year. As a reminder, these graphs only show albums from 2008 to 2019 (when the last EFY album was released).

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/length.png" alt = "Graph of lengths of songs (in seconds) for EFY albums from 2008 to 2019." style="width:500px;"/>

There's a strong mode of songs around 220-230 seconds (3:30-3:40), with over 25 songs in this range. The mean of the lengths also falls within this range (224 seconds, or 3:34). The data looks somewhat Normal, though there's a high concentration of points just below the mode and a larger spread of points above the mode. The shortest song, "We Stand" by Lauryn Judd (words and music by Nik Day) from the 2019 album, is 152 seconds (2:33), while the two outliers on the graph are both hymns in the Church of Jesus Christ of Latter-day Saints's hymnbook ("I Believe in Christ" from the 2009 album and "I Know That My Redeemer Lives" from the 2011 album), both sung by Jen Marco Handy and arranged by Michael R. Hicks. Both songs clock in over 360 seconds (6 minutes).

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/lengthyear.png" alt = "Graph of lengths of songs (in seconds) for EFY albums from 2008 to 2019, grouped by year or album (they're the same)." style="width:500px;"/>

The medians of these songs tend to hang around 225 seconds (3:15) - the most notable exception is 2010, where the median is nearly 250 seconds (3:40). Both of our 6-minute songs are outliers in their respective years, as is "We Stand." None of my chosen variables seem to be able to predict any commonalities between the outliers besides the two hymns, despite most albums having an outlier.

## Duplicates

One last graph for you: I noticed some repeat songs as I dug through the data (including one *very* notable one). Below are the reused songs, just for fun.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post4/dupes.png" alt = "Graph of number of uses of songs used on multiple EFY albums." style="width:500px;"/>

"Taking It Home with Me" is a song that's performed as a large group number on many of the '90s albums. Most of the rest of the songs are repeats, generally with new arrangements. I should note, however, that there is at least one instance of two different songs with the same name ("Light of the World"), so use these results with caution.

# What's next?

Next week, we dig into a deeper analysis on song length, as it relates to year and various credits. Stay tuned and let me know in the comments if there's anything else I should look into!