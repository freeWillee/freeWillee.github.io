---
layout: post
title:      "CLI Project - Doctor Ratings"
date:       2018-07-27 04:20:23 +0000
permalink:  cli_project_-_doctor_ratings
---



## Project Introduction

For my CLI project, I developed a simple application that lists a sample of physicians in the Calgary AB region.  The source of this information is via a website, RateMDs.com, which aggregates users’ reviews of medical practitioners.  It’s not exactly a primary source for health information (it functions similar to the popular rating/review site Yelp, Zomato, or Trip Advisor), but with enough user reviews, the information about doctors that the site is able to provide would at least be a good start for anyone that would like to begin their search for a physician in the city.  

Examples of details that can obtained are: 

* Doctor name
* Biography
* Specialty
* Sub-specialties
* Areas of interest
* Accepting new patients
* Reviews of doctors

As user friendly as the website already is, I decided to create a very primitive user interface that would automatically list a page of doctors, to be able to show key information only and not overload the user with ALL details of the doctor unless explicitly asked for.  Users will be presented with an option to drill further into a doctor to view details, or they could see a list of doctors sorted by specialty.

The CLI is by no means a complete solution for allowing a brand new user to find doctors but hopefully it will serve as a skeleton of a program that has the potential to be developed further with technologies I have yet to learn.  I can already imagine the many ways of slicing the data and returning very specific user defined options.  

Note for the purposes of this very simple application, the source page for this information was already defined to filter out “verified” doctors in the Calgary, AB region, and *only the first page of results* returned.  Hopefully, I’ll have the chance to modify and scale this in the future, but for the purposes of meeting my initial requirements for the project, this will suffice for now...

## Interface Walk-Through
After running the executable file in terminal, the user is prompted with a greeting message - 

`“Welcome to Calgary GPs!”`


What happens next was an intentional design choice on my part, at least for this particular version of the program — I decided to preload all of the details of the doctors that I want defined. 

The interface lets the user know that the doctors are being loaded (i.e. Name, specialty, and ratings are being scraped from a predefined BASE_URL).

Next the details of the doctors are loaded in another scraper method that happens behind the scenes (the source page is different from the method above; the details of the doctors are scraped from each individual doctor’s profile page).

A list of doctors along with general information is displayed and formatted for readability.  Each doctor has a list number assigned to him or her.  The user is then given options to do any of the following: 

1. Drill down to a specific doctor to learn more information by selecting the number corresponding to the doctor; 
2. Type ‘list specialties’ to display a sorted list of doctor by specialty; or
3. Type ‘exit’ to quit the program.

Edge cases are considered so I’ve programmed it so that any other option other than the ones described above will display an error message and force the user to try again until something valid is inputted.

When a specific doctor is selected, the interface displays a message that outputs: 

`Here is a sample list of Calgary's medical practitioners:`

For simplicity’s sake and on this version of the program, if the information is available, the biography of the doctor is displayed as well as whether or not he or she is accepting new patients; the above is formatted for readability.  Because no other information is available at this level of the program, the user is prompted to hit any key to return to the main menu in which the user is presented the default menu options from above.

When ‘list specialties’ is inputted by the user, the interface displays a message that outputs: 

`Here are a list of doctors by specialty`

All of the specialties of the first page are displayed as header with each doctor under a particular specialty shown as a list under the heading.  Again, as no other information is available, the user is prompted to hit any key to return to the main menu in which the user is presented the default menu options from above.  I would’ve given the user, at this point, the ability to drill into the doctor to show the page of the doctor selected, but ran out of time to finish that part of the code!

And finally if the user types ‘exit’, the program terminates and that’s the end of it!

## SOURCE CODE WALKTHROUGH

### File Structure

The project is setup as follows:

![Imgur](https://i.imgur.com/NfKyaCA.png?1)

### Executable and bin folder

The executable file is in a file of the same name as the application and is stored in the bin folder.  The file simply runs 

`CalgaryGps::CLI.new.run`

### CLI - the main body

The CLI source code file, located under* ./lib/calgarygps*, is where most of the code is run.

The `#run` simply calls a defined ordering of methods to serve the purpose of the application.  These methods are: 

```
#welcome_screen
#make_doctors
#list_doctors
#main_menu
```

#### CLI methods - an overview
The following is a brief description of what these methods do.

* The `#welcome_screen` is puts out a greeting message.

* `#make_doctors` first creates a hash of doctor information via a defined Scraper class.  The hash is then passed into a class method `.create_from_collection(doctor_hash)` of the defined Doctor class.

* `#get_doctor_details` essentially iterates through all Doctor objects that were scraped from the `#make_doctors` method above and further scrapes the individual doctor's profile_url page for additional details, namely biography and whether or not they are accepting new patients.  This is accomplished using metaprogramming - `.send("#{key}=, value)`.

* `#list_doctors` simply formats and lists all of the doctors stored in the class variable, @@all, which is an array of Doctor objects.  Each doctor object has instance variables containing details such as name, specialty, rating, biography, and whether they are accepting patients.  Everything except the last two attributes are listed when this method is run.

* `#main_menu` provides the user with options of what to do next.  Using `gets.strip`, the user input is stored in a variable which is then fed into a conditional if/else statement that first tests to see if a doctor was selected (because integers are used to drill into a particular doctor), and the else statement contains a conditional `case` statement to cover off all other user inputs, including edge cases.  When "exit" is typed, the program exits; when "list specialties" is typed, `#list_specialties` is run; when anything else is typed, the user is prompted with a message to try again and the `#main_menu` is run again.

#### Other classes
The defined classes used in this program include the Doctor class, the Specialty class and the Scraper class. 

The Doctor class is responsible for the creation and maintaining of doctor objects.  Each doctor attribute is simply a string that is scraped directly from the BASE_URL, aside from the specialty instance variable.  Each doctor's specialty is itself a Specialty object.

The Specialty class in turn is responsible for instantiating and maintaining all specialties.  Each specialty has a name and an array of doctor (objects) associated with it.  

## Conclusion
So that is a high level overview of my CLI project.  It was an extremely rewarding and satisfying experience.  It's my first time building an application from scratch, and though it is still extremely primitive, I am excited to build on this using tools I have yet to learn.  Thanks for making it this far!  Happy coding!





