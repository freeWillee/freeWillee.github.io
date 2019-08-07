---
layout: post
title:      "Complete React Project"
date:       2019-08-07 15:01:09 +0000
permalink:  complete_react_project
---



React is a wonderful framework.  Having built web applications using plain old Javascript with a combination of Ruby on Rails, the transition to React is quite seamless.  What is different however is how to think about the layout of the client side of the application.  The modular nature of React components helps to isolate specific aspects of your project, and it really helps to solidify the concept of the singular responsibility principle.  

Having said all that, I am excited to provide a brief description of the following web application, Cohort Plus that utilizes a React client side and a Rails api back-end.  

## Project Modifications Overview

The last iteration of this project saw a pure javascript front-end for one particular resource - the projects screen on the admin page.  With React, I have completely moved what was previously in the Rails views (using ERB) to using React.  As such, I have completely redesigned the layout of some of the screens including the user index page, the projects page, and the user dashboard.  All of the above are currently in its skeleton state.  I will be spending more time in the coming months to round out some of the features that I previously had in the rails project and come up with a fresher interface for this project.  

Already, utilizing a single page application, the screens load very fast and provides a seamless transition between the various screens and menu options at the top.

## Key features implemented
#### REDUX

In managing the various states of each component that needs it, I've had to make the conscious decision of what to put in a global state versus what to put in a local state.  Anytime I needed to pass certain attributes around to other components in the component tree because I needed to test whether or not the user was logged in for example, I needed to a global state to manage all of this.  Enter REDUX.  React redux is the global state management package I've opted to use for this project because it was relatively easy to implement and plays very nicely as middleware for the React project.  Amongst other things, I'm using Redux for the following: 

* Holding onto a list of all projects from the backend database for quick loading;
* Holding onto a list of all of the logged in user's tasks from the backend database for quick loading;
* Whether or not a modal should be displayed and particular screens to show (i.e. tell React which component to load in the modal - whether or a delete user or edit user screen).
* Whether or not a user is logged in (to dictate which component to load -- a dashboard or a homepage)

#### Thunk
Thunk middleware needed to be used in order to make backend queries to the Rails api when certain actions (in redux) are fired.  For example, typically an object would be returned when a particular action is dispatched, and the reducer's code is run -- the type of the action and payload if applicable is fed into the reducer which then takes over and modifies the global state accordingly.  With the thunk middleware, a callback function (with dispatch as an argument) is returned which in turn allows an asynchronous fetch request to be made to the rails back end, and once resolved, another action is dispatched to ultimately update the global redux state.  I have heavily relied on this middleware to make calls to the rails backend to ensure that the global redux state is constantly uptodate and displaying the right data.

#### Third party styling
This project also heavily relies on the @material-ui package (from Google) to help with overall styling of certain pages, namely the project screen, tasks screens, user index, and login.  There are a few higher order components that were used to help with defining the styling of certain @material-ui components, so that was something that I had to learn as well in implementing this.  

#### React Router
To create the feel of navigating through multiple pages in a single page application as well as telling React which component to load on a particular redirect, I've used the popular React-Router library to assist with routing.  Wrapping the entire `<App />` in the `<Router />` component starts things off.  Further defining the routes using `<Route component={CHOSEN_COMPONENT}` or the render attribute if needing to pass props around, is used pretty liberally to define the routes.

## What is next:
This project was fraught with a lot of trial, error, syntax errors, learning all sorts of different libraries.  As with my previous projects, it is tempting to spend countless hours refactoring, redesigning, adding things to redux, taking things out of redux, further splitting out components, not to mention add features.  This is a bit much at this point, but I can see now what is possible with just a few tools mentioned above.  I will be working to further refine this as I go and will continue to post updates as I am able to.

Similar to previous projects I have posted above, a note about the current version: 

To limit the possibility of scope-creep for the purposes of this project, I have only implemented minimal features to get the project off the ground -- a user can log in, create a new ID, create new tasks and projects.  Overtime some of these features will be limited to admin users - which I have yet to implement.  

## Coding the modifications

While the implementation currently works as I had originally anticipated I understand that there is so much more that I can do to clean up the code and make it more scalable.  

The source code goes way deeper than what I've described above so I encourage you to have a look through the repository [here (for the React frontend)](https://github.com/freeWillee/CohortPlus-client) and [here (for the Rails back-end)](https://github.com/freeWillee/CohortPlus-client) and please leave any comments for improvement.

