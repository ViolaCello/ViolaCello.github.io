---
layout: post
title:      "How I Stopped Being a Hater and Learned to Love Rails "
date:       2020-10-18 16:55:26 -0400
permalink:  how_i_stopped_being_a_hater_and_learned_to_love_rails
---

> "If you want to fly in the sky, you need to leave the earth. If you want to move forward, you need to let go the past that drags you down."
-- Amit Ray

After completing the module for Ruby’s Sinatra, I felt like I’d be coding like Bill Gates by the next morning.  The routing was so clear, straightforward, and may I say, intuitive.  Eight weeks of slugging away at Ruby object methods and SQL database queries left me wondering if I had it in me to proceed in a software engineering bootcamp.  Then along comes Sinatra with its CRUD functionality so apparent: GET this, POST that, PATCH this up over here, DELETE that over there.  ActiveRecord making database searches so clear that I felt like I was just asking the computer in my native  language what I wanted as if I were ordering a pastrami sandwich at the deli.      

And all the while I kept hearing, “If you love Sinatra, just wait until you work with Rails – it's like Sinatra with Magic!” What I wasn’t ready for was that Rails was actually the *devil’s magic*.  Rails to me was the legend of the Monkey’s Paw.  “We’ve made routing so simple for you in Rails that it goes where you want all by itself!”  Which in practice was as unnerving as ordering a car service, getting in the backseat, then realizing you’re in a driverless car.  It should be ok, but you typed in 55 Main Street, of which there are thousands of 55 Main Streets in your country and you’re not sure which one it thinks it should drive to. 

Three days into my Rails project and I had to do something about the anxiety and frustration.  From the notes I took during the lectures, there usually seemed to be a` byebug` commented out at the beginning of each method in the Controllers.  And in a rare moment of clarity, it came to me: What would we check once that method was hit? 

***It’s the params! ***

Armed with this recovered knowledge, I got in there.  I dropped `byebug` all over the place in my Controller methods.  Ok, what do the params look like?  Ok, here’s the` :id`  and it matches the `:id` of the View that it was coming from.  How can I apply this to what I know about Sinatra?  In Sinatra, we'd write `get '/user/:id/edit' do` but in Rails, this was simply: `def edit` and now I realized how Rails knows the rest: We are already in the UserController, therefore, no need to write `get /user` whilst the :id is still in the params, so Rails already knows the `/user/:id` and since the method is called `edit` it just completes this for us.  Now, `get '/user/:id/edit` becomes `def edit`.   Same thing on a redirect.  In Sinatra, it was `redirect "/profile/#{@profile.id}/edit"` whereas Rails can now handle this with: `redirect_to user_edit_path`.  Why did I hate this?

## Don't be Afraid of Scope

Another thing that orginally drove me mad in Rails: Scope methods.  Why do we need scope methods?  They don't do anything that a class method couldn't do.   I reluctantly re-wrote my Venue Class method which searched for venues by state, originally:
```
 def self.by_state(input)
     self.where("state = ?", input)
 end
```

Why should I refactor this to a scope method?  It wasn't apparent until I did it the first time:
```
scope :by_state, ->(input) { where("state = ?", input) }
```

Ok, one line, easy to see what it does, and its close to the top of the model file.  I'm beginning to see.  I did this a few more times in my Events model:
```
  scope :upcoming, -> { where 'curtain > ?', DateTime.now }
  scope :past, -> { where 'curtain < ?', DateTime.now }
```

Pretty easy to see what I'm up to with these methods.  Very easy to read.  I see you, Scope Methods...

And what's even more fun is that now, all on one line, I can call:

**Venue.by_state(input).first.events.upcoming.order(:curtain)**

where Event belongs_to a Venue, which has_many Events.  Iterating over this in the Views was very clear and smooth now.

## Time for Action:  Enter the `before_action`

How many times in Sinatra did I type `@user = User.find_by(id: params[:id])` ?  Almost every method in my UserController began with this line, or had this line of code somewhere in there.  Now, with a simple private method: 

```
def get_user
@user = User.find_by(id: params[:id])
end
```

and a ```before_action :get_user, except: [:index, :new, :create]``` near the top of the UserController, I can pull all this redundant code out of my methods, out of my GitHub storage, and out of my life.  

Working on this project helped me to appreciate Rails.  And I don't even get me started on `render partial:` for keeping it DRY.  

This taught me a lot about myself.  Letting go...letting go of the relationship I had with Sinatra, a very controlling relationship on my part, which has now opened me up to how much better a relationship can be when I open up to trust - the trusting relationship that I now have with Rails.  *(and when I don't trust Rails, I can always check the params! Shhhh)*

