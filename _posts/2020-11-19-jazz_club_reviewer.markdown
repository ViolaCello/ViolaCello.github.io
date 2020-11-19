---
layout: post
title:      "Jazz Club Reviewer"
date:       2020-11-19 17:43:14 -0500
permalink:  jazz_club_reviewer
---



This is a Jazz Club Reviewer which uses a Rails API backend with a JavaScript-based frontend.

There are two models in the database:
  Clubs in which there is a :has_many relation ship with Reviews (where the Reviews :belongs_to the Club).
  
 All interactions between the client and the server are handled asynchronously (AJAX) and use JSON as the communication format.
  -- There are three (3) AJAX calls to the backend database using fetch()
    1) The first of these is handled upon loading.  There is a GET request to the ClubsController Index route which returns, in JSON format, all of the Club.all information AND nested within this, is also the Reviews which belong to each Club.
    2) There is a POST request to the ClubsController Create route which instantiates a new Club into the database on the backend, which, once the information is returned, a new instance of the Club Object is created in the Club Class in JavaScript to keep the user experience flowing and as to not have to send another fetch() GET request to the database
    3) There is a POST request to the ReviewsController Create route which instantiates a new Review once a user submits a comments form.  The club_id is taken from the information on the DOM and sent to the database with the appropriate club_id for proper identification.  It is then added to the pre-existing Club Object in the JavaScript Club class
    
 The JavaScript application uses Object Oriented JavaScript (classes) to encapsulate related data and behavior.
    This includes constructing a new instance of Club upon rendering the Clubs already in the database, as well as when a user creates one in real-time
    There are static methods (such as the renerAll() upon loading/initalizing) as well as class instance methods like averageRating() which calculates the average star ratings that a particular Club has stored in the database.
		
## Inside the Project
All along the way, there were certain decisions that needed to be made once I knew what kind of project that I wanted to create.  Besides the basics of table columns and their relationships, I needed to know how I would structure the frontend JavaScript file(s) so that the code wouldn't get chaotic.  Almost immediately, it did...

###  Take the Time to Plan the JavaScript Classes, /or/ Refactoring at the end may never end...

Once I got the basic HTML and CSS templates down that I created from scratch, I was eager to get my first `fetch()` request happening.  But since I wanted to "seed" my database by actually using the app, the first thing I did was get the **Add Venue** functionality working, which means I needed to set-up a POST request first.  This worked out, so then I wanted to have the GET request working to populate the DOM with information from the database over in Rails.  After this was working, I realized my code was almost 200 lines long in the `index.js` file.  Now what?  

### API Class


    
    
  
