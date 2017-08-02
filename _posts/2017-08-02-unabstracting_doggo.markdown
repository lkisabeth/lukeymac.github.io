---
layout: post
title:  "UNabstracting Doggo.."
date:   2017-08-02 17:50:15 +0000
---


Today, I found out that trying to make a project LESS abstract is really frustrating and confusing.  For my first Rails portfolio project, I decided to try to push my boundaries and add a bunch of AJAX magic to the app.  This was important to me at the time because I thought the app would have felt terrible without it. I still feel that way.. but I found out too late that I can't use the same JS implementation in my Rails JS Assessment.  I used the `remote: true` solution for all of my AJAX goodness and OF COURSE the requirements for the JS Assessment explicitly state "Do not use `remote: true`."  AWESOME.

So, how do I meet the requirements for this project without completely tearing down my old one?  Good question.  I'm still trying to figure it out!  So far, I've decided to keep SOME of the `remote: true` features and refactor just enough of the others to meet the requirements.  Here's the plan for each of the requirements:

*Must render at least one index page (index resource - ‘list of things’) via jQuery and an Active Model Serialization JSON Backend.*

Render all barkbacks on their respective howl divs without refreshing the page.

*Must render at least one show page (show resource - ‘one specific thing’) via jQuery and an Active Model Serialization JSON Backend.*

Below each howl, put a button that will retrieve and append a representation of the howl from a json call.

*The rails API must reveal at least one has-many relationship in the JSON that is then rendered to the page. *

The JSON versions of each howl appended at the bottom will contain a 'has-many' relationship with barkbacks.

*Must use your Rails API and a form to create a resource and render the response without a page refresh.*

Create a new barkback this way.  Use handlebars for templating.

*Must translate the JSON responses into Javascript Model Objects. The Model Objects must have at least one method on the prototype.*

Barkbacks and Howls with both be converted to true JS Model objects within their respective .js files.  Create a basic prototype method for Howls.

That should do it!  I'll update this post or create a new one with everything is done!

Happy Coding <3

Luke
