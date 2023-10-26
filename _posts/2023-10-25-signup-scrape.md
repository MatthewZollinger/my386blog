---
layout: post
title:  "What's Next for SignUp on Disney+? Part 1: Scraping"
author: Matthew Zollinger
description: Selenium and the Sorting Algorithm
image: /assets/images/post7/bluey2.png
---

A few months ago, Disney+ released a new version of the Marvel Cinematic Universe film *Ant-Man* with American Sign Language (ASL) captioning ([here's a full article about it](https://whatsondisneyplus.com/disney-adds-asl-version-of-marvels-ant-man/)). I took two ASL classes during my senior year of college (read: I was still in one of those classes when this version of *Ant-Man* released), so this announcement caught my attention. I was curious to see what other films on Disney+ had ASL captions, so I searched the Internet and found little else provided by Disney themselves. Luckily, I *did* find another solution: [SignUp](https://www.signupcaptions.com/), a company that produces free ASL captions.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post7/signuphome.png" style="width:800px;"/>

I highly recommend checking out their site, but here's the short version of what SignUp's all about: they have interpreters do full interpretations of movies or TV episodes and then make them available via a free browser extension that deploys a popup YouTube video when you select a compatible title. You can also view all of their titles from the extension itself; it's nifty and easy to use, as long as you're okay with watching on your computer (at the time of writing, SignUp is working on expanding device compatiblity). Right now, you can use SignUp with your United States, United Kingdom, or India accounts of Disney+, Hotstar, Netflix, or Peacock.

You may be wondering at this point what all this has to do with data analysis. I was interested in investigating what factors all of SignUp's supported Disney+ titles have in common, and see if I could use data to make some predictions about what will be added next!

## Scraping SignUp

Getting the supported list of Disney+ titles was mostly pretty easy, but it did present me with one challenge: the list itself is on SignUp's homepage, but you have to click a button that says "View all titles" to access and scrape it.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post7/signupbutton.png" style="width:800px;"/>

This issue led me to investigate Selenium, a Python package that allows us to automate interactions with elements of a webpage. Here's the code I ended up using:

```
# initializes our Edge session
driver = webdriver.Edge()

url = "https://www.signupcaptions.com/"
driver.get(url)

# a wait condition
# it waits until the CSS selector of the button we want to click is present before moving on and times out after 3 seconds
WebDriverWait(driver, timeout = timer).until(EC.presence_of_element_located((By.CSS_SELECTOR, "#movies > button")))

# tracks down the button we want to click and stores it
b = driver.find_element(By.CSS_SELECTOR, "#movies > button")
# clicks the button
driver.execute_script("arguments[0].click();", b)
# waits until the list of movies is visible before moving on and times out after 3 seconds
#"#movies > div.allMovies > div:nth-child(62) > img"
WebDriverWait(driver, timeout=timer).until(EC.visibility_of_element_located((By.CSS_SELECTOR, "#movies > div.allMovies")))
# saves the new version of the page as a variable
exp = driver.page_source
# closes the driver
driver.quit()
```

This took me quite a bit of time and searching the Internet to figure out; I'll briefly break down what this code does. First, it initializes a driver object that is capable of opening a web page. Unlike with the requests package, I can actually see this page open up. After passing the URL to the driver, I have a wait implemented. My code can move quicker than the web page can load, so I have to use this wait to give the "View all titles" button time to load (its CSS selector is "#movies > button"). Then the code assigns the button (again through its CSS selector) to a variable, uses a command to click it, then waits again until the list of movies is visible. After that I can save the page and close up.

From here all I did with the page was grab those movies, as well as the sign languages for which captions are available. You can find my full code in [this GitHub repo](https://github.com/MatthewZollinger/SignUp-Disney-Info) (the file's called "SignUp Scraping.ipynb).

## Flixable

Getting our SignUp list is only half the story, however (or maybe less) - we need data on both our supported and unsupported titles. Rather than scraping Disney+ itself, I decided to use [Flixable](flixable.com/disney-plus) to get our info.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post7/flixable.png" style="width:800px;"/>

I chose Flixable for a few reasons - first, because I knew I'd need to sign in to Disney+ if I scraped it. That's possible, but I was already learning quite a bit about Selenium without adding automated login to the mix (maybe watch for that in a future post)! Additionally, Flixable gives us a few extra bits of info, including IMDb score and the date a title was added to Disney+. Flixable also seems lighter on the processor than Disney+ is, which is good, because getting a full list of current titles takes *quite* a bit of effort for my poor laptop.

You see, Flixable's list of all Disney+ titles (accessed by putting nothing into the search bar) *loads dynamically*. This once again calls for Selenium; here's some code that I partially adapted from [this Stack Overflow response by Ratmir Asanov](https://stackoverflow.com/a/48851166):

```
# Get scroll height.
last_height = driver.execute_script("return document.body.scrollHeight")
refresh = False

while True:

    # Scroll down to the bottom.
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

    # Wait to load the page.
    sleep(5)

    # Calculate new scroll height and compare with last scroll height.
    new_height = driver.execute_script("return document.body.scrollHeight")

    if new_height == last_height:
        if refresh:
            exp = driver.page_source
            break
        driver.refresh()
        refresh = True
    else:
        refresh = False
    last_height = new_height

# closes the driver
driver.quit()
```

This code scrolls to the bottom of a page, waits for a few seconds, then tries to do so again. If it does, it keeps going until it won't scroll anymore (as happens on Flixable, at least on my computer, despite it not reaching the last Disney+ title). Once it's unsuccessful in reaching a new minimum height, it refreshes the page and does this same routine all over again. When it refreshes, the page is able to keep loading more titles if there are any. To keep the code out of an infinite loop, the `refresh` flag is set to True each time the code has to refresh, and if it doesn't hit a new minimum height, the code concludes we've hit the end of the list and saves the page. If it does hit a new minimum height, the `refresh` flag is set to false and we proceed.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post7/atlantis.png" style="width:800px;"/>

Most of the rest of the scraping is straightforward - the code scrapes the links to individual titles' pages from this page, then scrapes each title's page for its name, IMDb rating, release year, content rating, runtime, and date added to Disney+. It also scrapes for genre - and here I have to acknowledge that currently, Flixable only shows the first three genres alphabetically, while Disney+ can display more than three genres and doesn't limit itself to alphabetical order. As an example, where Marvel Studios' *Loki* is listed as "Science Fiction, Fantasy, Super Hero, Action-Adventure" on Disney+ itself, Flixable displays it as "Action-Adventure, Fantasy, Science Fiction," with no mention of it being a superhero show. Correcting this error is the main reason I'm considering scraping Disney+ itself.

I should also mention the sorting algorithm I devised for sorting out genre. I wanted each genre to be a variable on its own in our final data frame. The genres being in alphabetical order actually helps here; my algorithm takes a list containing each genre, starts with the first, and checks its first letter against letters in alphabetical order. Here's a sample of the code:

```
if (current[0] == "A"):
        if (current == "Action-Adventure"):
            GAcAd[i] = True
            current, curnum = success(current, curnum)
        if (current[1] == "n"):
            if (current[2] == "i"):
                if (current == "Animals & Nature"):
                    GAnimals[i] = True
                    current, curnum = success(current, curnum)
                if (current == "Animation"):
                    GAnimation[i] = True
                    current, curnum = success(current, curnum)
                if (current == "Anime"):
                    GAnime[i] = True
                    current, curnum = success(current, curnum)
            if (current == "Anthology"):
                GAnthology[i] = True
                current, curnum = success(current, curnum)
    if (current[0] == "B"):
        if (current == "Biographical"):
            GBio[i] = True
            current, curnum = success(current, curnum)
        if (current == "Buddy"):
            GBuddy[i] = True
            current, curnum = success(current, curnum)
```

Sometimes I have to go as deep as three letters before checking to see if we have a perfect match in genre, but once we do, we mark the respective entry in the genre's list as `True` and then advance to the next genre in the list. While this does require a minimum of 16 checks per title, the algorithm never repeat checks for each title because it never loops back to the beginning until it has proceeded to a new title. (If you have suggestions for improving this algorithm, let me know in the comments here or on LinkedIn!) You can find my full code in [the GitHub repo I mentioned earlier](https://github.com/MatthewZollinger/SignUp-Disney-Info) (this file's called "Flixable Scraping.ipynb). The output CSV files are also in that repo.

<img src ="https://raw.githubusercontent.com/MatthewZollinger/my386blog/main/assets/images/post7/hocuspocus.png" style="width:800px;"/>

So, now that we've got two data sets, we can run the numbers and see what trends we notice between sign-captioned and non-sign-captioned titles. Look for that post coming soon!



