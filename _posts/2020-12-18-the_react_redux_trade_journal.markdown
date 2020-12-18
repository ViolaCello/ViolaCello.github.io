---
layout: post
title:      "The React/Redux Trade Journal"
date:       2020-12-18 21:45:04 +0000
permalink:  the_react_redux_trade_journal
---


A journal is a must-have for when undertaking any serious endeavor.  Among the journals I personally keep, my notebook for the FlatIron Software Engineering program is a living journal of my progress through the world of coding.  When I first walked through the doors of a professional securities trading jobs, the first thing we learned on the first day of trading was how to keep a trade journal.  I personally was never satisfied with any of the electronic journals for logging trades that were available.  Therefore, when my final project at FlatIron came around, I felt had the skills to develop the trade journal that I’ve always wanted.  

Redux was well suited for this project because of its “store” being a single source of truth for the data.  Once my initial fetch() request loaded the data into the store, I was able to grab that data anywhere in the app by connecting the component to the store via react-redux and the mapStateToProps function.  

The main feature that sets this journal apart from the others is the feature of assigning a strategy field to each logged trade.  I had always wanted to find out what strategy was working best at a glance from my other journals, but this was never easy to do.  In React-Redux, I was able to drop all of the formulas I needed to compute the statistical analysis into a single Utilities file and call on these functions when needed.

The biggest challenge in this project was getting components to render onto the DOM conditionally.  This was very easy in vanilla JavaScript due to the many DOM manipulation functions and identifiers available.  In React, this was a bit more involved and often involved creating a toggle using local state.  But, I’ve learned how to accomplish this, and I feel that going forward, I’m looking to really dive deep into React.

