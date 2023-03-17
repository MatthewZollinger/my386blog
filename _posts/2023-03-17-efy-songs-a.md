---
layout: post
title:  "EFY Album Data Part 1: Collection"
author: Matthew Zollinger
description: Scraping from SingPraises.net (and YouTube) is a simple snap!
image:
---

Ah, EFY albums. The savior of many a Latter-day Saint missionary's sanity, these albums are religious in focus and contemporary in tone. I love them - maybe a little too much, since not only do I have a goal to collect them all, but I'm also doing data analysis about them - specifically, whether certain credited artists tended toward longer or shorter songs.

## What is EFY?

Especially for Youth (EFY) is an organization operated by Brigham Young University that runs retreats centered around standards of the Church of Jesus Christ of Latter-day Saints. EFY has operated for decades and, from 1984 until 2019, released religious songs (usually a full album) on a CD that attendees received as part of their admission to the program.

In 2019, the Church announced that it would be sponsoring For the Strength of Youth (FSY) camps that were to be a key part of its new Children and Youth program. These camps are much cheaper than EFY camps were and are, and FSY camps somewhat supplanted EFY camps in function. EFY still exists today - it still operates FSY camps, in fact, as well as special camps at areas with importance to the Church. Albums still accompany these camps, but they are the Church's freely downloadable youth albums (which have been made annually since 2009) rather than being a separate album to accompany just the camp's themes.

Not that you'll find many people who make this distinction - to be fair, Church youth albums and EFY albums have many common contributors and tones - but right now, my focus is just on EFY albums. (And I nitpick!)

<img src ="https://github.com/MatthewZollinger/my386blog/blob/main/assets/images/efy2007.jpg" alt = "An image of an EFY album, Power in Purity: EFY 2007. Also the entirety of my collection." style="width:400px;"/>

## The Data

I generated my data set from two websites:

1. [SingPraises.net](singpraises.net)

SingPraises.net is a website operated by Samuel Bradshaw. It's dedicated to documenting songs of or related to the Church of Jesus Christ of Latter-day Saints, which naturally includes EFY songs. From SingPraises.net, I collected album titles, song titles, credits for each song, and a YouTube link to a legal upload of a recording where applicable.

As SingPraises.net's [robots.txt](https://singpraises.net/robots.txt) allows for the scraping of any pages that aren't parameterized versions of its collections or search pages, I was able to simply scrape URLs from a page with all of the EFY albums and scrape each of those URLs for album data.

2. [YouTube](youtube.com)

You might have heard of this little site before. Remember the legal YouTube uploads of EFY recordings? After scraping those URLs from SingPraises.net, I went on to scrape each of *those* URLs for the length of the video to get track length. (In the file, track length is in seconds.)

YouTube's [robots.txt](https://www.youtube.com/robots.txt) file has a few more restrictions, but none surrounding the videos I wanted to scrape from (which have a "watch" suffix with additional parameterizations). Thanks to [others on the Internet](https://stackoverflow.com/questions/33698776/how-can-i-get-the-duration-of-youtube-video-with-python), I was able to track down the duration of a video from the HTMl and put it in.

I also found this fun note on YouTube's robots.txt file:

<img src ="https://github.com/MatthewZollinger/my386blog/blob/main/assets/images/YouTube.png" alt = "A screenshot of Microsoft Edge on a Windows computer open to youtube.com/robots.txt. The text says 'Created in the distant future (the year 2000) after the robotic uprising of the mid 90's which wiped out all humans.' YouTube wasn't even around in the 90s, right?" style="width:400px;"/>

## Next Steps

Well, scraping the data took a little bit, but now I have it in a CSV file! You can access a GitHub repo with the CSV and the code used to scrape it [here](https://github.com/MatthewZollinger/EFY-Album-Data).

Look forward to my next post, in which I dissect this data and answer the age-old question: **Kenneth Cope or Nik Day?** Who wrote the longest songs? (And probably some other questions too.)