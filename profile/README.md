# Filmer Portfolio

# Table of Contents


# Project Description 
Everyone knows the problem: when you finally get the time to sit down and watch a film, you suddenly have no idea what to watch, and spend half your evening
mindlessly browsing Netflix. 

Filmer is designed to solve this issue: it offers a simple and easy-to-use interface that can recommend you dozens of movies in minutes, all based on your 
personal tastes. If the recommended movie doesn't seem to be your thing, you can just say you're not interested and the next recommendation will immediately 
be shown.

[Recommendations Gif]

If something comes up that sounds interesting, but you're not quite in the mood for it right now, you can save it to your watchlist, so 
you'll have a personalized list of recommendations within arm's reach at all time. Here, you can always check your recommendations, and once you've seen a film
you can indicate what you thought of it, further refining your personal recommendation algorithm.

[Watchlist Gif]

## Technical Details
The Filmer website is built in React, which communicates through endpoints with the backend API. There are multiple backends: one for managing account details 
registering, and logging in, and one to manage a user's watchlist and recommend new films. The backends themselves are written in C#, and use Entity Framework
Core to manage their SQL Server databases.

For a full list of technical specifications, as well as justifications for my choices, see [Technical Choices]

## Group Project
Alongside my personal project, I worked with a group on a different application: Eeventify. Eeventify is a social app that allows users to organise activities 
and events, which can found and joined by other users. This way you'll never be looking for people to play football, watch a movie, or just hang out again.

# Learning Outcomes
## 1: Full-Stack
>### _You design and build **user-friendly**, **full-stack** web applications._
[User Friendly]

Like mentioned before, the Filmer's frontend is written in React, and makes use of a set of APIs. Through a variety of endpoints, the app can access
a user's saved watchlist, allow them to make changes to it, and recommend films not yet on the user's list.

[Auth0 Login]

The database is fully handled through Entity Framework Core, through a code-first approach. An algorithm in the backend, all written in C#, picks out film
recommendations based on the films already on the user's watchlist.

As for Eeventify, it uses the same technology. Its frontend is also written in React, and it also uses a C# backend with Entity Framework Core. These 
choices were partially made because our team was most comfortable with them: especially when it came to C# we already had some experience. Using the same
technologies also ensured that I didn't have to learn additional programming languages, which saved me a lot of time, especially considering the fact that
I already had to learn to use both Entity Framework and React.

## 2: Software Quality
>###  _You use software **tooling and methodology** that continuously monitors and improve the software quality during software development._
My application uses a variety of testing techniques, all of which are automatically executed at various stages of deployment, to ensure the accurate
monitoring of software quality.

The first layer of testing is through unit tests, written in the backend in the NUnit framework. These tests mostly focus on the backends film recommendation 
algorithm, because it contains the most complicated logic of the application. Whenever something is pushed to the main branch of the backend github repository,
Github Actions is used to automatically build the application and run all unit tests, the result of which is displayed on the repository's front page. This
way, I can always be sure my algorithm is functioning.

[Github actions testing]

A second layer of testing happens through Postman. I created a suite of integration tests that ensures my endpoints are in working order, all of which are 
collected in a monitor which runs all of the tests once every 2 hours, so I'll always get a timely warning that something about my application has stopped 
working. Unfortunately, monitors don't work when an application is hosted on localhost (even through Docker), which means that they currently always fail.
If I set up a server, however, the tests would start working with only minor changes to their urls.

[Postman monitor]

Eeventify was hosted on an actual server, so the monitors did function there.

[Postman Eeventify]

## 3: Agile
>###  _You **choose** and implement the most suitable agile software development method for your software project._
For our group project, we used the Scrum method; a streamlined working environment was especially important because it wasn't just our group working on this
project. There were two different groups of students from Finland who made a frontend that made use of our backend, so timely deliveries and good communication
was essential. Scrum was a good fit for us, because it allowed us to set clear goals for when features that the other groups needed would be finished.

We used a rotating Scrum master, and supplemented the method by using a sort of Kanban board, that gave use a better overview of the work that was finished,
in development, or still had to be done.

[Taiga board]

While not perfect, I think our group worked very well together, aided by agile: especially towards the end, we made a lot of progress and managed to meet all 
of our goals.

## 4: CI/CD
>###  _You **design and implement** a (semi)automated software release process that matches the needs of the project context._
Both parts of my backend, and my frontend, are containerized using Docker. All of this happens automatically whenever something is committed to the main branch
the respective repositories, once again using Github Actions. When something is pushed to main, the program automatically gets built, and a Docker container is
created that gets updated on my Docker Hub. This way, I can easily run my application through Docker and ensure the most recent version is running.

[Docker Hub]

For Eevenify, we use a very similar setup: whenever an update is made to a service, the corresponding Docker Container is updated and ran on the server.

## 5: Cultural differences
>###  _You **recognize** and **take into account** cultural differences between project stakeholders and ethical aspects in software development._
The largest difference in culture I had to contend with this semester was our group's collaboration on Eeventify with the students from Oulu, Finland. Not only
were they from a different country, they were also from a different University with constrasting ways of working and a differing yearly schedule. I do
think there were some challenges starting out, especially since were designing a backend for two seperate frontend teams, but a lot of those kinks got worked
out very well, and in the end I think our collaboration transpired very smoothly.

As for ethical aspects, I have carried out research on the ethics surrounding the collaboration and communication with other groups, especially within an
educational setting:
[Ethics Research]

## 6: Requirements
>### _You analyze (non-functional) requirements, elaborate (architectural) designs and validate them using **multiple types of test techniques**._
For Filmer, I tried to plan out the overall project structure as well as possible. I originally wanted to split up the backend in as many microservices as 
possible, but during development I unfortunately ran into some issues that made me decide to merge some of the services, ending up with the two I have now. For
more info, see the [Technical Choices] chapter.

For Eeventify, we did end up compartmentalizing the backend a lot more, with the multiple services being routed through an API gateway. In general, I think we
planned out this project quite well, extensively discussing our setup before starting work, which helped greatly with maintaining a well organised project from
the start.

[Eeventify diagram]

We had an interesting challenge when it came to Eeventify, because we essentially had three different stakeholders: the groups from Oulu, ourselves, and our 
teachers. During the first part of the semester, we largely focused on the groups from Finland. We got a prototype of the backend up and running as fast as
possible, and then started adding more and more features based on what the other groups required. During this process, we obviously tested out our own prototype,
but also tried to set up automatic testing, and we frequently got feedback from the frontend groups when something needed to be implemented, or when an existing
feature was broken, not functioning as intended, or simply confusing to use. We tried to respond to this feedback as quickly as possible, considering the other
groups were depending on our backend for their own application's functionality.
At the same time, and especially once the groups from Finland had finished their project, we tried to implement features we had personally planned and ones our
teachers wanted to see (not only programming tasks, but also research, ethics considerations, etc). After we had finished our collaboration, we built our own
frontend making use of our backend, quickly setting up a prototype and incrementally adding more features to suit our own and our teachers' needs.

We did also discuss security features within the group, although we haven't yet managed to implement any of them.

For Filmer, I feel like I messed up quite badly when it comes to the teachers asstakeholders (see the [Semester Reflection]). I did however try gather as 
much feedback from my friends as possible, first interviewing them on features they would want to see in an app like this and using their feedback to 
create my requirements, and once the app was up and running inviting them to mess around with it to identify bugs, which helped me find a couple of things 
I had overlooked.

[Docs interview results]

[Requirements]

## 7: Business Processes
>###  _You analyze and describe **simple** business processes that are **related** to your project._
(Research will be added later)

## 8: Professional
>###  _You act in a **professional manner** during software development and learning._
Like I said, I really feel like I dropped the ball when it comes to my personal project, but I do feel like developed professionally during the work on 
Eeventify. None of us had a lot of experience with building a full-stack application like this one, so we had a lot of discussion on how to design it and what
technical choices would be optimal for it, not only within our own group but also with the groups from Finland, which resulted in a (in my opinion) quite well
set-up end result. We also frequently helped each other understand parts of the technology we didn't understand yet, which helped greatly improve our working
speed.

# Technical Choices and Other Considerations
## C#

## EFC

## React

## Microservices

## UX

# Research
I researched what the ACM Code of Ethics had to say about working together in a group, and how that code applied in our way of working, and in an educational
setting:  
[Ethics Research]

I researched the optimal choice of Database and ORM framework for Filmer:  
[Database & ORM Research]

I researched cryptography and how it could be applied to increase the security of both Filmer and Eeventify:  
[Cryptography Research]

# Semester Reflection

# Links
[Technical Choices]

[Ethics Research]

[Cryptography Research]
<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
