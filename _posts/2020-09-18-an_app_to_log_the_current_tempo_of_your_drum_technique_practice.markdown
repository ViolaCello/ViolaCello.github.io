---
layout: post
title:      "An App to Log the Current Tempo of your Drum Technique Practice"
date:       2020-09-18 18:29:42 +0000
permalink:  an_app_to_log_the_current_tempo_of_your_drum_technique_practice
---


> Arriving at one goal is the starting point to another. – John Dewey

For my second project at the Flatiron School for Software Engineering, I created an app where you can log the tempo (as expressed in beats-per-minute, also known as BPM), and also see what other users are practicing and at what tempo.  The community feature of this app isn’t necessary for a user to engage in, for this can be used only for your own online practice log.  However, the idea of community always appeals to me for many reasons, not the least one being that the healthy social pressure to show up and log a practice, knowing that others (potentially) could be watching.  But for myself as a musician, whether anyone else could see it or not, I have always wanted a way to electronically log into a database what technique I’ve been practicing and what my current BPM I’m currently able to perform at.

This app uses Sqlite3 and ActiveRecord to manage and store the user’s data.  The Corneal gem was used to set-up the MVC file tree template and optimize it to be able to take advantage of the Sinatra library of code built into Ruby.

Before beginning to set my Models (which up to this point in my knowledge, were class object in Ruby), I had to decide the relationship between my classes and tables.  I knew I needed some for of User Class that would set and store a drummer’s information, including biographical data and login information.  I would need a class/model of practice techniques that a typical drummer would be practicing daily, which I decided to call Lessons.  There would also need be a table that stored where the user’s practice information would be logged.  I called this `join table` Goals, since it would store the user’s unique database id, the Lesson’s unique database id, as well as the current tempo and current tempo goal for this particular practice.  The user’s table would be called the Drummers since the users are drummers after all.  

The relationships between the classes were simple to see one I had the diagram in place, and I knew what I wanted to accomplish.  The Drummer would have many Goals.  The Lessons would have many Goals.  Each individual Goal would belong to both a Drummer and a Lesson.  Each Drummer would have many Lessons through Goals, and each Lesson would have many Drummers through Goals also.  This is a classic has many to has many through configuration.

The programming was rather straightforward once I had the table relationships drawn-up.  Having done this before I started typing any code helped tremendously in keeping database `rollback migrations` to a minimum.  Since many drummers practice similar rudiments and combinations, I decided to pre-populate the Lessons table with seed data that each user could choose from.  Once a drummer logs in, they can edit any of their previous logged data, create a new entry, or delete any logged entries, as well as edit the data on their profile page, and even delete their account entirely.

The navigations between my `controllers` and the `views` was very clear to navigate.  However, I would like to bring-up the biggest challenge of this project: Compromised data, and iterations over it.  I added a feature for the user to be able to delete their account.  This is simple to accomplish.  But due to a feature I decided to incorporate into the views – a Leader’s Page and a Community Page – both of which allow other users and even non-users to view at will, there arose the situation wherein a drummer_id would be deleted entirely from the database, leaving a `nil` value for the rows in the Goals table where the drummer_id would be.  This in and of itself would not be catastrophic, except that when a Views page would iterate through the Goals information and hit a method that called upon `Goal.drummer_id` the `nil` value would crash the program.  Since I had already programmed many different conditionals in the views pages, I decided that the best way to handle this unwanted phenomenon was to iterate through the Goals Class at the start-up to remove any rows from the goals table that did not contain both a `drummer_id` and a `lesson_id` to keep the app from crashing.  Also, from a practical standpoint, why keep data from a user who deleted their account showing up on the Community Page and the Leaders Page (if they made it to the Leaders Page)?  Also, as a way to make sure a user’s data was deleted from the entire database once they clicked the button to delete their account, right in the controller that handled the `delete` request.

In the future, once we move onto learning Ruby on Rails, I would like to add a Comments Class to the Community page so that people can comment on other drummer’s progress and perhaps offer some practical tips, making this a more robust social/community experience.

If you would like to see a short video of the app in action, here is the link: [https://youtu.be/ejKy0gR7RRY](http://)

Below are the two .perge class methods in the Goal Class.

```
def self.purge_by_id(drummer) # this removes a drummer's data once account is deleted
    g = Goal.all
    g.map do |i|
        if i.drummer_id == drummer
         i.delete
        end
    end
end

def self.purge_unidentified_data
    g = Goal.all
        g.map do |i|
            if !i.drummer_id || !i.lesson_id
                i.delete
            end
        end
```

