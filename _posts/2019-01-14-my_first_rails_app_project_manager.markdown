---
layout: post
title:      "My First Rails App: Project Manager"
date:       2019-01-14 21:24:31 +0000
permalink:  my_first_rails_app_project_manager
---


## Project Introduction

This application is a mini project management system for project administrators to be able to create, modify, add, and delete projects.  Within each project will be a series of tasks that can be assigned to any number of users.  Users, in turn, will have the ability to update the status of any task that is assigned to them.  

The genesis of this project came from discussions with various project managers that had many projects on the go with multiple tasks and users to manage.  Every user could be involved in multiple projects and to keep track of all of the moving parts was a difficult.  Also there needed to be a medium in which a user was able to communicate with a project manager at anytime in the least amount of steps, and vice versa.  Too much time was spent figuring out each day where each project was, which matters were urgent when each project was due, etc.  This application is a first step in attempting to meet the goal of being a one stop place to manage a project manager's workload and making it easier for team members as well.

As you can imagine, there are infinite number of features that can be implemented, but for the purposes of this project, I have decided to focus on the following key points:

1) The project manager is the 'admin' user that can control everything about the project, including user login credentials (over time this should simplified, and delegated accordingly, mostly because it  would be too easy to accidentally delete database entries).
2) Only the admin can add and destroy projects. 
3) A user can create his or her own account on Facebook; before the user can start anything, an admin must assign them to a project.
4) An admin may not necessarily know which project to assign a task to (e.g. they just thought of a task that needs to be done but doesn't know who to assign to or if the task will overlap with multiple projects, etc.).  In this case, the admin can use an "unsorted project".
5) After a project has been assigned, a user (not an admin) can go into the project and add or edit their own tasks to that specific project.  The user can only add to or edit a task of a project that they are already assigned to.

This first version of this project manager app will be accessed through a webpage, complete with login credentials required to access a user’s page.  The user’s page will show a list of projects assigned to the user, and the tasks under each project.  

With the assistance of the Omniauth gem, users will be able to login their account using Facebook.  Future versions and updates to this project will allow for login using Google, Twitter, or other popular social media platforms.

A note about the current version: 

To limit the possibility of scope-creep, I have intentionally left out certain functions.  For example, in the first version of this application, only the admin user account is able to fully CRUD projects and tasks.  In the future, a user will be able to access these features as long as they are granted 'admin' status on their user account.  Another feature is the use of filters and sorting.  While this iteration of the project only allows for the filtering of users for a particular task, a feature in the future will allow for a user to quickly sort through existing tasks on their dashboard by project deadlilne or task completion status.  This feature would also be useful for the admin user to see.

## Interface Walk-Through

### Logging in / Signing in
Verifications are in place to ensure that if a user tries to create a login with a username that already exists in the database, the user is presented with error messages indicating as such.  A user can create a username and password by clicking on the signup link on the main page, or login via Facebook.

If the admin account is user, the top of the page will indicate as such, otherwise a user sees their project dashboard page showing the following: 
1. Outstanding tasks
2. Current projects

### CRUD
A user can only add a task to a project that they are assigned to.  Once a project has been assigned to them, the user can add or edit the task (name, details, completion status).

Only an admin logged in as such can create, edit, update, and destroy projects, users, and tasks.

### Tasks
A task will have a unique user, and a project.

A user can see all their tasks in one table, showing the completion status, and a link to the related project.
A user can edit each task, updating the task name, details, and completion status.

An admin can add, edit, and destroy tasks.  Only an admin can destroy a task.  

### Projects
A project can have many tasks, and will have many users through the tasks that make up a project.

A project will have a name, and a deadline.  When an admin creates a project, they can immediately assign to the project, any combination of users in the database.  Because a project has users through tasks, a default task will be assigned to each user, appropriately titled 'pending'.  After project creation, the admin will see the project main page, a list of tasks assigned to any combination of users, and the ability to edit the tasks as needed (i.e. change the title, details, and completion status).

When a project is deleted by an admin, all associated tasks are also deleted.

### Users
A user has many projects through tasks assigned to them.  

A user can be created using Facebook login.  When this is done, the user defaults to having nothing assigned to them. When a user is created in this fashion, the admin will have to go into the admin console to assign projects / tasks to them.  Note that when a user creates an account this way, the default password for the sake of validations is "default".

When the admin creates the user, the admin can do so in the admin console.

Only an admin can delete a user, and when this is done, all associated tasks are also deleted.

## SOURCE CODE WALKTHROUGH

### Database and Models
Using ActiveRecord, migrations were created to form 3 tables.  Corresponding models were created with the following associations:

1) Project - has many tasks, has many users through tasks
2) Task - belongs to a user, belongs to a project [this is the joins table]
3) User - has many tasks, has many projects, through tasks.

The User models also utilizes the Secure Password ActiveModel to ensure that passwords are appropriately hashed when stored on the database.   

#### Validations: 
The following validations are setup in this application: 

User:
* Must have a unique username
* Must have a unique email

Task:
* Must have a title

Project:
* Must have a name
* Must have a deadline that is not in the past

Once the database and models were appropriately setup, I played around in the console to ensure that the objects are appropriately defined and were accurately associating themselves with each other.

### Controllers
In ensuring RESTFUL routes were created, separate controllers were defined to handle unique tasks as they related to a particular object model.  Thus controllers were defined along with key routes as follows: 

#### Sessions
A route for Facebook, a route to login, a route to create the session, and a route to logout
```
  get '/auth/facebook/callback' => 'sessions#fblogin'
  get '/login' => "sessions#new"
  post '/sessions' => "sessions#create"
  get '/logout' => "sessions#destroy"```
	

#### Projects
A nested route for projects seen by users.  Users can only view a particular project (namely the ones that they have been assigned to): 
```
  resources :projects, only: [:index, :new, :show] do
    resources :tasks, only: [:new, :create, :edit, :update]
  end```

#### Tasks
Users can only add and edit tasks.
  ```resources :tasks, only: [:new, :create, :edit, :update, :show]```

#### Users, nested tasks
Users will be able to see their tasks that are assigned to them.  A nested route will show an list of all tasks for the user.
A nested route will also show all projects assigned to the user.  

If a user wishes to drill down to the individual task or project, the user will be redirected to the route of that particular project or task

```  resources :users, only: [:new, :index, :show, :edit, :update, :create] do
    resources :tasks, only: [:index, :show]
    resources :projects, only: [:index, :show]
  end```

### Admin routes
Because the admin can manage all aspects of the application and because I wanted to route with a module and use that module for the url prefix, I have used the namespace method (rather than scope, module).

```  namespace :admin do
    resources :users
    resources :projects
    resources :tasks
  end```
		
### Views
Views are set up for all CRUD actions for the admin.  In addition, a separate layout for admin was created to show only the nav links that an admin can access (namely the CRUD actions for a user, task, and project).  

The user will see a separate view (and layout) for the actions that are restricted for them (only add and edit tasks, add/edit users,  and view only projects).![](![](http://)http://)

CSS is used for the project, copied from FlatIron's Amusement Park project.		

The source code goes way deeper than what I've described above so I encourage you to have a look through the [repository](https://github.com/freeWillee/project-manager_001) and please leave any comments for improvement.

## Conclusion
So that is a high level overview of my very first Rails project. It was an extremely rewarding and I can see the fruits of my labour coming into view.  I've come a very long way from "hello world".  I thrive on curiosity so this is just the beginning.  Thanks for making it this far and journeying with me!
