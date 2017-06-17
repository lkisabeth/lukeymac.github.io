---
layout: post
title:  "My First Ruby Gem!"
date:   2017-05-05 21:35:17 -0400
---


Earlier today, I published version 1.0.0 of my first ruby gem to rubygems.org!  This gem also represents my first portfolio project for the Flatiron School.  I've really enjoyed the labs throughout the curriculum but there is something special about finishing your own project; something you put together yourself from scratch.  The gem is called "latest_games" and you can try it out on your machine by running 'gem install latest_games'.  It scrapes data from metacritic.com about the latest video games. The lists are broken down by platform and the current iteration will show you the publisher, release date, other systems the game is available on, metascore, and a brief summary.  If that sounds interesting to you, check it out!  I would appreciate ANY feedback!

The weeks leading up to today have been a bit crazy!  My girlfriend and I just finished a move this week from Columbus to Zanesville, OH.  It was often difficult to find the time to code while we were packing, planning, and packing some more.  This left me worried that I was getting too far removed from my studies and would lose some of what I had just learned and fall behind.  Once we got settled a couple days ago, however, I was ready to set my sights back on programing.  Fortunately, I regained confidence in what I was doing pretty quickly and was able to push through finishing the project.  It feels good to be back!

Now, let's talk about the gem more specifically.  This was quite the learning experience!  The motivation behind this particular gem was my passion for video games.  My longterm goal is to work as a programmer in the video gaming industry.  I figured this was a good place to start!  I'm always searching the internet for the latest games and reviews.. so I thought it would be cool to be able to do that right in the terminal!  

The first issue I ran into was having two "list views"; first displaying a list of platforms to choose from and then displaying a list of games for the platform of choice. It sounded really simple at first but it took me some time to figure out the best way to organize the code.  I ended up creating Classes for each of the platforms' games (PS4Game, PCGame, iOSGame, etc.)  This way I could let each Class contain it's own scraper with a platform specific URL for the list of games.. and it would allow some modularity for platform specific feature changes.  When you select a platform from the first list.. you are only instantiating objects for games specific to that platform.. not all games on the site. The data scraped for each of these game objects include their own URL which is used to scrape the remainder of their own data, once chosen.

It's important to note, with this approach, that there is some redundant code across the game Classes.  Regardless of platform, indiviual games have mainly the same xpaths for their data (like publisher and release date).  This bothered me at first but I decided to leave it this way because it would give me the flexibility in the future to change features for each platform without risking interference with the others.

One thing that bothered me was the short load time when scraping a list of games.  I set out looking for a visual solution for this and discovered a handy little method for a spinning "Loading" symbol.  I tweaked it a bit to add the "Loading.. " text.. here it is:

```
def wait_cursor(seconds, fps=10)
    chars = %w[| / - \\]
    delay = 1.0/fps
    (seconds * fps).round.times{ |i|
      print "Loading.. " + chars[i % chars.length]
      sleep delay
      print "\b\b\b\b\b\b\b\b\b\b\b"
    }
end
```

In my opinion, this resulted in a more enjoyable visual experience when running into load times.

I could probably ramble a lot more about this CLI gem but this blog post is getting pretty long!  If I make changes to the program I'll try to update you in another blog post!

Best wishes and happy coding,

Luke Kisabeth
