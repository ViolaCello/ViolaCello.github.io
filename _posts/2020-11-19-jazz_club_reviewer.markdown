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
    There are static methods (such as the renderAll() upon loading/initializing) as well as class instance methods like averageRating() which calculates the average star ratings that a particular Club has stored in the database.
		
## Inside the Project
All along the way, there were certain decisions that needed to be made once I knew what kind of project that I wanted to create.  Besides the basics of table columns and their relationships, I needed to know how I would structure the frontend JavaScript file(s) so that the code wouldn't get chaotic.  Almost immediately, it did...

###  Take the Time to Plan the JavaScript Classes, /or/ *Refactoring at the end may never end...*

Once I got the basic HTML and CSS templates down that I created from scratch, I was eager to get my first `fetch()` request happening.  But since I wanted to "seed" my database by actually using the app, the first thing I did was get the **Add Venue** functionality working, which means I needed to set-up a POST request first.  This worked out, so then I wanted to have the GET request working to populate the DOM with information from the database over in Rails.  After this was working, I realized my code was almost 200 lines long in the `index.js` file.  Now what?  

### ApiService Class

Since these would basically function similarly (they will all begin with "http://localhost:3000/" for starters), this made sense to have as its own class.  This would also have the added benefit of keeping all of the `async` and `await` keywords always involving the APIService class.  It was simple enough to set-up in the JavaScript as well, since at the top of the code, there was just instantiating a new instance of the ApiServices class, which I was then free to use for the remainder of the code.  For example:
```
const api = new ApiService();


const init = () =>{
  renderClubs()
  
}

// DOM identifiers
const leftPage = document.querySelector(".column1")
const rightPage = document.querySelector(".column2")

// add list of all clubs onto the page upon receiving the info from the database
async function renderClubs(){
  const clubs = await api.getAllClubs()
  for(club of clubs){
    new Club(club)
  }
  rightPage.innerHTML = ""
  rightPage.innerHTML = Club.renderAll()
  selectClubToView()
}


init()
```


And with this piece of code opening up the `index.js` file, we already have quite a bit done:
* We have waited (see `await`) for the communication to the database to return the information that we wanted (ie., all the data for all the clubs)
* We have set the two major identifiers of where we want to display into onto the DOM (ie., Column1, Column2) as variables
* Most importantly, have taken our Clubs object class in JavaScript and created a new instance of each Club coming back from the database, which came in handy later on when adding Reviews to these Clubs and also when clicking on them to get more information from them

Whereas when I first learned about JavaScript Object Classes I was like, "Oh no...what are we going to need this for?"  I now have seen how they can help clean-up the code, which at the end of the day, makes it both easier to read and to write.
  
