---
layout: post
title:      "Sinatra Web App: Patient-List"
date:       2018-10-16 17:39:25 +0000
permalink:  sinatra_web_app_patient-list
---


## Project Introduction

This Sinatra web application that I've developed is a prototype for a patient database management system.  The application is meant to be used in a medical clinic setting with multiple physicians working in the clinic, and a centralized administrative system.

Users (i.e. physicians) will create their own unique login with a username and a password.  After creating a login, the user will be able to add patients to his or her profile.  In its current form, the application only allows for primitive patient information as follows: 
    1) Name
    2) Birth Year
    3) Phone
    4) Address
    5) Health Care Number
    
Users will be able to create patient histories for any particular patient.  The history record belongs to the patient who in turn belongs to a specific user, and it is only the specific user that will be able to login, access, read, update, and delete the patient corresponding histories. 

In addition, the main patient list page will show a list of active users in the clinic and number of patients that any particular user has.  This is a blueprint with future extendability to account for the maximum capacity of the clinic and for the individual user.  Further, the user will in the future be able to have specific attributes that define the physician, e.g. specialty, subspecialty, gender, years in practice, etc.

## Interface Walk-Through
The homepage is a generic view of users and an anonymous patient list that only shows the count of patients that belongs to the user.  This is meant to serve as a high level overview of how many patients are currently in the clinic and, in the future, will extend to show the capacity of the clinic as well as for the physician users.

### Logging in / Signing in
Verifications are in place to ensure that if a user tries to create a login with a username that already exists in the database, the user is presented with error messages indicating as such.  A user can create a username and password by clicking on the signup link on the main page.

Once signed in, the user is brought to the patient summary page.  A new user will not have any patients yet so it will be up to him or her to create one using the create new patient link.  An existing user can login by clicking on the login link on the main page -- once logged in, the existing user will be brought to his or her patient summary page.

### Viewing, Editing, Deleting
A particular patient will have generic details (in this prototype of a web app) such as : name, birth year, phone, address, and health care number.  The user will only be able to edit patients that belong to him or her.  The user can edit the patient only when the user dives into the patient detail page.  The user can edit or delete the patient from here. 

On the patient detail page is also the history summary of the patient.  A bird's eye view of the history records are available on this page, with the option for the user to click on any particular history to view the details (i.e. the detailed notes taken on that history entry).  From that detail history screen, the user can edit or delete the history record similar to the patient record above.

### User controls
There are validations in place to ensure that another user or non-logged-in user cannot view, edit, or delete any patient or history record of a patient or history that doesn't belong to them.  

## SOURCE CODE WALKTHROUGH

### Database and Models
Using ActiveRecord, migrations were created to form 3 tables.  Corresponding models were created with the following associations:

1) History - belongs to a patient
2) User - has many patients
3) Patient - belongs to a user & has many histories

The patient models also utilizes the Secure Password ActiveModel to ensure that passwords are appropriately hashed when stored on the database.   Validations are also a part of the patient model to ensure that on the creation of a user, the username is unique and a password is mandatory.

Once the database and models were appropriately setup, I played around in the console to ensure that the objects are appropriately defined and were accurately associating themselves with each other.

### Controllers
In ensuring RESTFUL routes were created, separate controllers were defined to handle unique tasks as they related to a particular object model.  Thus controllers were defined along with routes as follows: 

1) UsersControllers
    a) get '/signup' => visit signup page for new user.
	  b) post '/users' => to create a new user and re-route to patient list page.
		
2) SessionsControllers
    a) get '/login' => to present user to login page.
	  b) post '/sessions' => to login in the user, save the session, and redirect user to patient list page.
		c) get '/logout' => to logout user by deleting the clearing the current session and redirecting the user to the generic patient list page.
		
3) PatientsControllers
    a) get '/patients' => shows the patient list page or generic patient list page depending on whether the user is logged in.
	  b) get '/patients/new' => to create a new patient for the logged in user.
		c) get '/patients/:id' => to show the patient detail that the user clicks on.
		d) post '/patients' => to create a new patient, save to database, and redirect to patient page.
		e) get '/patients/:id/edit => to show the edit form for the current patient
		f) patch '/patients/:id/edit => to edit the patient and save to database, and redirect the user to the edited patient detail page.
		g) delete '/patients/:id/delete => to delete the particular patient from the database and redirect user to the patient list page of the user.
		
4) HistoriesControllers
    a) get '/patients' => shows the patient list page or generic patient list page depending on whether the user is logged in.
	  b) get '/patients/:id/histories/new' => to create a new history for the particular patient.
		c) get '/patients/:id/histories/:history_id' => to show the particular patient history detail that the user clicks on.
		d) post '/patients/:id/histories' => to create a new history for the particular patient, save to database, and redirect to history page.
		e) get '/patients/:id/histories/:history_id/edit => to show the edit form for the history record of the particular patient
		f) patch '/patients/:id/histories/:history_id' => to edit the patient history and save to database, and redirect the user to the edited patient history page.
		g) delete '/patients/:id/histories/:history_id/delete' => to delete the particular patient history from the database and redirect user to the patient detail page of the user.
		
### Views
There views set up for histories, patients, users, and sessions.  Each view renders the appropriate form using .erb, and html tags to apply primitive styling for tables.  No CSS is used for the purposes of this project.  A generic layout.erb is created to show on every rendered page.  The layout.erb has yields to the relevant view that is rendered from the user clicking on any particular route.		

## Conclusion
So that is a high level overview of my Sinatra project. It was an extremely rewarding and I can see the fruits of my labour coming into view.  I've come along way from "hello world".  I thrive on curiosity so this is just the beginning.  Thanks for making it this far and journeying with me!
