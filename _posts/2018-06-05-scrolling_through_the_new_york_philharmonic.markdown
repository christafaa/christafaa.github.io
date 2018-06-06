---
layout: post
title:      "Scrolling through the New York Philharmonic"
date:       2018-06-05 18:39:59 -0400
permalink:  scrolling_through_the_new_york_philharmonic
---

For my CLI Data Gem Project, I created an application that scrapes all upcoming New York Philharmonic concerts from [this page.](https://nyphil.org/calendar?season=all&page=all)

The first challenge I ran into was linking a concert's date with the concert itself using the list page pasted above. It's not the case for every date, but there are some days with *multiple* concerts. With the current HTML layout, if I grabbed all dates and all concerts, I would end up having more concerts than dates - how would I match those up so they were correct? Here is a visual example of the problem:

June 5th, 2018
* Concert #1

June 6th, 2018
* Concert #2
* Concert #3

...more dates...

The problem here is June 6th. By that point, I already have more concerts than dates. Additionally, dates and concerts aren't stored in the same HTML "block" so I wouldn't be able to iterate over a section of concerts and refer to their date. Since I couldn't think of a creative scraping solution for this and I felt it was important to include a concert's dates in the initial list in the CLI, I decided to only collect concert page URLs from the main list and then scrape all concert attributes from each individual concert page. Fine, but this presented another challenge: load time. 

The NY Philharmonic currently has 163 listed concerts. Some of those concerts are repeated but I think it's safe to say there are over 125 unique concerts listed. If I scraped every unique concert page, it would take 20 minutes just to load the program. 

To mitigate this issue, I designed a scrolling system in my CLI. I created a Page class that stored 5 concerts in each page instance. When the program starts, I create the first page and display it. The user can then "turn the page" which would create a new instance of a page which would store 5 newly created concert instances. By creating only 5 concert instances at a time, and only when the user wants to see more concerts, I was able to get what I wanted (concert titles displaying with the correct concert dates) at the expense of a slight load time. At least when a page is created, it doesn't need to be reloaded. 

Check out upcoming NY Phil concerts here: [Github Repo](https://github.com/christafaa/ny-philharmonic-cli-app)


