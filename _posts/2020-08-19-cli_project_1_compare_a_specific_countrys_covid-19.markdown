---
layout: post
title:      "CLI Project 1: Compare a Specific Country's Covid-19 "
date:       2020-08-19 20:56:53 +0000
permalink:  cli_project_1_compare_a_specific_countrys_covid-19
---



> Intelligence consists in ignoring things that are irrelevant.
                                - Nassim Nicholas Taleb

One pleasant side benefit of immersing myself in learning to code in the Summer of 2020 is that finally there is something that distracts me for long stretches of the day (and often into the night) from this novel coronavirus.  Even better, coding diverts my attention away from the ever-present chatter about it.  I do take seriously the medical considerations of becoming infected with Covid-19.  My brother was on his back for three weeks when he caught it.  The same goes for my sister-in-law, who works as a nurse in the ER.  It is not something that you want to catch by any stretch of the imagination.  Therefore, in order to just get down to the bare facts of the matter, I found an API that can generate reported Covid-19 data for particular countries, and another API that delivers daily reporting Covid-19 data.

The parameters of the project were specific: 

* Write the code in the Ruby language
* Use either API or scrapped web data
* Use at least three classes
* Avoid repeating your code (also known by the acronym DRY).
* Use iterations

Two of the three necessary classes were easily conceived of: There would need to be an API Class that would fetch the live data, and there would be a CLI Class where the user would interact with the available data via the terminal.  What I was having trouble with was the third necessary class, and this was because I could not envisage this third class without an additional fourth class.  I had incorrectly assumed that my Information Class would also function as the "work flow" class, giving the instructions to the CLI as to what should be done after each menu level.  This premise necessitated a fourth class that would story all the data of each country, called a Country Class.  However, after having my Information Class continually trip over itself, to the point where I began referring to it as the Misinformation Class, it dawned on me a thought which has now become my first coding axiom:

> If the name of the class that you designed has become an antonym of itself, remove it.
                           - Tony Lepore

Now I had my workflow happening.  The API would collect the data from the web as per its function, the Country Class would receive the data in the form of a hash and store that information into class variables for the relevant data (which for this project is cases Confirmed, Recovered, Critical, and Deaths), which would be called upon from the CLI Class if the user requested it.  The CLI would also handle the workflow, since so much of what the program needed to do depended upon the selections from the user, who interacted directly with the CLI Class.

After some trial and error, it was functioning as it should.  However, I quickly bored of the raw numbers that the CLI was able to generate for me.  What did these numbers mean anyway, if not for a comparison to another dataset.  I decided that the best way to achieve some sort of relevance with these numbers would be to compare them with the total world Covid-19 numbers.  This information I found in a different API (but from the same API family).  The first step would be prompt the user to answer whether they would even care for this comparison.  Then if so, what subset of numbers would be compared?  This did not take too long to implement, since I had the world data populate once the program started running so that it would be at the ready for comparison at any moment.  

The part that really gave me trouble was getting a usable percentage number the data I already had.  I am not accustomed to having a computer return to me an inaccurate calculation.  But in Ruby, 50 divided by 100 somehow equals zero.  I tried attaching `.to_f` to the end of my calculation.  Ruby responded by mocking me with the return of `0.0` - obviously I was dealing with a wiseguy...

After spending way too long on trying to get a way for this line to return a number like 0.72 or 1.44
```
percentage = ( 100 * (stats.confirmed / @@world.confirmed))
```
I reached out for help to my instructor and cohorts with the issue.  For purposes of anyone else who may run into the same issue, in Ruby, one must attached a `.to_f` to each individual integer, so that my final code looked like this:

```
        local = (stats.confirmed).to_f
        world = (@@world.confirmed).to_f
        percent = (100 * (local / world))
        puts ""
        puts "#{stats.country} has #{percent}% of the confirmed cases in the world."
```

There were two other things that I learned specifically while doing this project that I'm certain to use in the future.  One of these is the `Heredoc` which allows one to embed multi-line strings in your code.  This turned out to be very useful when creating my welcome message and menu items, which removed the need to continually add a `puts` on every line.

The other thing I learn while exploring for simplicity throughout this project, was the `case` and `when` feature to cut down on multiple `if` `elsif` statements.  

Some things I can attempt in the future for this CLI is to find a way to return a more graphically pleasant display (I tried `require colorize` but I kept getting errors), as well as finding a way to generate commas within integers so that 1675283 looks more like 1,675,283 when displayed on the console.

However, I hope that Covid-19 will soon become just a faint blip in history and this CLI can be used for archival purposes only.
