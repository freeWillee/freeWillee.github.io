---
layout: post
title:      "Implementing JS"
date:       2019-03-28 15:44:49 +0000
permalink:  implementing_js
---


## Project Modifications Overview

Building on the previously implemented rails project, I have build a javascript front-end for one particular resource for now - the projects screen on the admin page.  The user experience is not slightly improved in that the admin user can now see at a high level the outstandign tasks of any particular project without refreshing the page.  

Also, when the user clicks on a particular project, the project is rendered on the screen, again without a page refresh thanks to an AJAX call to the backend, and the details of the project are shown on the screen.  The user can then cycle through all of the current projects quickly without a screen refresh.  

For any particular project, a user can click on a button, "create new task", to show a form on the screen right then and there.  After the details of the task are inputted into the form, the task is appended to the table on the project screen.

A note about the current version: 

To limit the possibility of scope-creep for the purposes of this project, I have only implemented the JS frontend for the admin account of the project resource only.  Future implementations will have a JS frontend and utilize AJAX calls where appropriate and practical to maximize user experience.

## Coding the modifications

This was by far one of the most challenging projects that I have done to date.  The initial challenge was figuring where best to implement AJAX calls without diminishing the overall user experience.  No amount of brilliant coding, refactoring, and feature implementation is useful unless the end user feels that it is something that makes the experience better.  Because the applicaiton was so large, I had to isolate only one section.  I decided at the end of it all to only implement an AJAX call as a "trial run" for a project resource for the admin, since it is the admin that has super user rights.

Switching gears between a recently learned Ruby language to javascript and knowing how to utilize both at the same time often caused a lot of confusion especially when it came to syntax.  I had to learn to isolate code and keep in mind at all times, where in the stack am i? 

I am still gaining valuable experience and insight into the importance of having properly organized code - to constantly thinking about DRY-ing up the code.  

While the implementation currently works as I had originally anticipated I understand that there is so much more that I can do to clean up the code and make it more scalable.  

The experience I've gained while working on this project for over 15 hours is invaluable.  The temptation is to ensure everything is perfect, but as I have learned quickly - there's no such thing as perfect code and there is always something to improve.  For now, this will do and I can check it off as one more step towards learning how to build a fully functioning web application using modern web technologies.  I'm excited for what's next...

The source code goes way deeper than what I've described above so I encourage you to have a look through the [repository](https://github.com/freeWillee/project-manager_002) and please leave any comments for improvement.

