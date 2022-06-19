# Filmer Portfolio
By Rik Jansen

# Table of Contents
- [Filmer Portfolio](#filmer-portfolio)
- [Table of Contents](#table-of-contents)
- [Project Description](#project-description)
  * [Technical Details](#technical-details)
  * [Group Project](#group-project)
- [Learning Outcomes](#learning-outcomes)
  * [1: Full-Stack](#1-full-stack)
  * [2: Software Quality](#2-software-quality)
  * [3: Agile](#3-agile)
  * [4: CI/CD](#4-cicd)
  * [5: Cultural differences](#5-cultural-differences)
  * [6: Requirements](#6-requirements)
  * [7: Business Processes](#7-business-processes)
  * [8: Professional](#8-professional)
- [Technical Choices and Other Considerations](#technical-choices-and-other-considerations)
    + [C#](#c)
    + [Entity Framework Core / Microsoft SQL Server](#entity-framework-core-and-microsoft-sql-server)
    + [React](#react)
    + [Testing](#testing)
    + [Microservices](#microservices)
    + [UX](#ux)
- [Research](#research)
- [Semester Reflection](#semester-reflection)
- [Links](#links)

# Project Description 
Everyone knows the problem: when you finally get the time to sit down and watch a film, you suddenly have no idea what to watch, and spend half your evening
mindlessly browsing Netflix. 

Filmer is designed to solve this issue: it offers a simple and easy-to-use interface that can recommend you dozens of movies in minutes, all based on your 
personal tastes. If the recommended movie doesn't seem to be your thing, you can just say you're not interested and the next recommendation will immediately 
be shown.

![Recommendations Gif](https://github.com/FilmerApp/.github/blob/main/images/Recommendations%20Gif.gif)

If something comes up that sounds interesting, but you're not quite in the mood for it right now, you can save it to your watchlist, so 
you'll have a personalized list of recommendations within arm's reach at all time. Here, you can always check your recommendations, and once you've seen a film
you can indicate what you thought of it, further refining your personal recommendation algorithm.

![Watchlist Gif](https://github.com/FilmerApp/.github/blob/main/images/Watchlist%20Gif.gif)

## Technical Details
The Filmer website is built in React, which communicates through endpoints with the backend API. There are multiple backends: one for managing account details 
registering, and logging in, and one to manage a user's watchlist and recommend new films. The backends themselves are written in C#, and use Entity Framework
Core to manage their SQL Server databases.

For a full list of technical specifications, as well as justifications for my choices, see [Technical Choices and Other Considerations](#technical-choices-and-other-considerations)

## Group Project
Alongside my personal project, I worked with a group on a different application: Eeventify. Eeventify is a social app that allows users to organise activities and events, which can found and joined by other users. This way you'll never be looking for people to play football, watch a movie, or just hang out again.

The repository for Eeventify is located [here](https://github.com/Eeventify).

# Learning Outcomes
## 1: Full-Stack
>### _You design and build **user-friendly**, **full-stack** web applications._

Like mentioned before, the Filmer's frontend is written in React, and makes use of a set of APIs. Through a variety of endpoints, the app can access
a user's saved watchlist, allow them to make changes to it, and recommend films not yet on the user's list.

Originally, I planned to implement my own login service, for which I built a seperate backend. Eventually however, I opted to use Auth0 for my login functionality. Not only is Auth0 easy to implement and comes with some nice quality of life features - like being able to create an account through your google account, it also has a lot of safety features that would be quite challenging to implement from the ground up. Auth0 features bot detection and CAPTCHA, brute force protection, and can detect users logging in with credentials that have been part of a data breach.

![Auth0 Login](https://github.com/FilmerApp/.github/blob/main/images/Login%20Gif.gif)

Once logged in, the user's ID is requested through my own API, which can then be used to load their watchlist and personalized recommendations.

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

![Github actions testing](https://github.com/FilmerApp/.github/blob/main/images/Automatic%20Unit%20Testing.png)

A second layer of testing happens through Postman. I created a suite of integration tests that ensures my endpoints are in working order, all of which are 
collected in a monitor which runs all of the tests once every 2 hours, so I'll always get a timely warning that something about my application has stopped 
working. Unfortunately, monitors don't work when an application is hosted on localhost (even through Docker), which means that they currently always fail.
If I set up a server, however, the tests would start working with only minor changes to their urls.

Eeventify was hosted on an actual server, so the monitors did function there.

![Postman Eeventify](https://github.com/FilmerApp/.github/blob/main/images/Eeventify%20Monitor.png)

## 3: Agile
>###  _You **choose** and implement the most suitable agile software development method for your software project._
For our group project, we used the Scrum method; a streamlined working environment was especially important because it wasn't just our group working on this
project. There were two different groups of students from Finland who made a frontend that made use of our backend, so timely deliveries and good communication
was essential. Scrum was a good fit for us, because it allowed us to set clear goals for when features that the other groups needed would be finished.

We used a rotating Scrum master, and supplemented the method by using a sort of Kanban board, that gave use a better overview of the work that was finished,
in development, or still had to be done.

![Taiga board](https://github.com/FilmerApp/.github/blob/main/images/Taiga%20Board.png)

While not perfect, I think our group worked very well together, aided by agile: especially towards the end, we made a lot of progress and managed to meet all 
of our goals.

## 4: CI/CD
>###  _You **design and implement** a (semi)automated software release process that matches the needs of the project context._
Both parts of my backend, and my frontend, are containerized using Docker. All of this happens automatically whenever something is committed to the main branch
the respective repositories, once again using Github Actions. When something is pushed to main, the program automatically gets built, and a Docker container is
created that gets updated on my Docker Hub. This way, I can easily run my application through Docker and ensure the most recent version is running.

![Docker Github Actions](https://github.com/FilmerApp/.github/blob/main/images/Automatic%20Docker%20build.png)

![Docker Repositories](https://github.com/FilmerApp/.github/blob/main/images/Docker%20Repositories.png)

For Eevenify, we use a very similar setup: whenever an update is made to a service, the corresponding Docker Container is updated and ran on the server.

## 5: Cultural differences
>###  _You **recognize** and **take into account** cultural differences between project stakeholders and ethical aspects in software development._
The largest difference in culture I had to contend with this semester was our group's collaboration on Eeventify with the students from Oulu, Finland. Not only
were they from a different country, they were also from a different University with constrasting ways of working and a differing yearly schedule. I do
think there were some challenges starting out, especially since were designing a backend for two seperate frontend teams, but a lot of those kinks got worked
out very well, and in the end I think our collaboration transpired very smoothly.

As for ethical aspects, I have carried out research on the ethics surrounding the collaboration and communication with other groups, especially within an
educational setting:
[Ethics Research](https://github.com/FilmerApp/.github/blob/main/Research/Ethics.md)

## 6: Requirements
>### _You analyze (non-functional) requirements, elaborate (architectural) designs and validate them using **multiple types of test techniques**._
For Filmer, I tried to plan out the overall project structure as well as possible. I originally wanted to split up the backend in as many microservices as 
possible, but during development I unfortunately ran into some issues that made me decide to merge some of the services, ending up with the two I have now. For
more info, see the [Technical Choices and Other Considerations](#technical-choices-and-other-considerations) chapter. I also created a C4 model for my application, see [Miscellaneous Design Diagrams](https://github.com/FilmerApp/.github/blob/main/profile/Design%20Documents.md)

For Eeventify, we did end up compartmentalizing the backend a lot more, with the multiple services being routed through an API gateway. In general, I think we
planned out this project quite well, extensively discussing our setup before starting work, which helped greatly with maintaining a well organised project from
the start.

![Eeventify diagram](https://github.com/FilmerApp/.github/blob/main/images/Eeventify%20Layout.png)

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

For Filmer, I feel like I messed up quite badly when it comes to the teachers asstakeholders (see the [Semester Reflection](#semester-reflection)). I did however try gather as 
much feedback from my friends as possible, first interviewing them on features they would want to see in an app like this and using their feedback to 
create my requirements, and once the app was up and running inviting them to mess around with it to identify bugs, which helped me find a couple of things 
I had overlooked.

![Docs interview results](https://github.com/FilmerApp/.github/blob/main/images/Interview%20Results.png)

[Requirements](Requirements.md)

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
### C#
One of the biggest technological choices to be made for both applications I worked on was the programming language the backends would be created in. For both, the choice fell on C#. My reasoning is as follows:
  * C# is a modern, frequently updated programming language, with many useful features like null-safety that something like Java lacks.
  * Programming in C# is relatively easy, but it is still a powerful language that allows you to 
  * C# is part of the .NET framework, which means you can easily integrate it with other frameworks like ASP.NET. There is a wide availability of frameworks for C# to serve many different purpose, for example Entity Framework Core (see below).
  * There is extensive support and documentation for C#, and Microsoft has a range of well-written tutorials on pretty much any subject to do with the .NET framework.
  * Both the rest of my group members and I had the most experience using C#
### Entity Framework Core and Microsoft SQL Server
I researched the best options for Database Management Systems and ORM here:
[Database & ORM Research](https://github.com/FilmerApp/.github/blob/main/Research/Database%20and%20ORMs.md)
### React
The choice for Javascript framework was possibly the most important one this semester. Going into the semester, I had never used Javascript in any way, so I would have to learn to use Javascript and a framework from scratch. 
  * Like I said, I had no experience using Javascript before this semester. React's JSX syntax has a lot of similarities to HTML, a language which I do know, which makes the learning process easier.
  * There are a lot of well-made tutorials teaching React.
  * React's focus on reusable components is very powerful when your application features lists of repeat elements, like mine does. The way it handles inheritance is also familiar to someone coming from an object oriented background.
  * React's biggest downside is that it natively only supports single-page applications. There are, however, additional libraries for routing - I used React Router.
### Testing
I outline the details of my testing setup in more detail in [Software Quality](#2-software-quality), but I wanted to have a variety of testing methods to cover as much of my application as possible. NUnit is a framework I have experience using, and it is well suited to testing C# backend logic, which is especially important with my recommendation algorithm. I chose Postman to test my endpoints because of its ease of use, making it a perfect choice for trying out the API after I had my prototype set up. Postman is also able to periodcally run tests, which helps greatly with automating endpoint testing.
### Microservices
Like mentioned before, I tried to set up microservices for Filmer, and we attempted to do the same for Eeventify, with mixed success. Microservices offer a lot of benefits:
  * Because microservices are completely seperate, it is very easy to make changes to a single services without having to redeploy the rest of your application
  * One or a few members of a team can build and mantain a single microservice, which makes development easier and more agile. We really noticed the benefits of this way of working in our group project.
  * Microservices can be scaled individually, which means that if one service sees way more traffic, it can be scaled independently from the others.
However, we ran into a major challenge: managing our data. Our microservices often needed to access the same data, and giving them all their own seperate database would mean that data would go out of sync when edited. There are solutions for this (implementing an event bus, like Azure Service Bus), but they are quite complex and were unfortunately not within the scope of this semester. In the end, we opted to use endpoint calls between the different microservices for Eeventify, which worked, but ruins many of the advantages the microservices could offer. For Filmer, I eventually decided to merge the services that used the same data to make development more manageable.
Below is the original intended setup for the project - the Watchlist and Recommendation services have been merged.

![Microservices diagram](https://github.com/FilmerApp/.github/blob/main/images/Component%20Diagram.png)
### UX
I tried to keep User Experience in the back of my head throughout the development of Filmer. Right at the start, I collected feedback on why and how potential users would want to use the app, through interviews conducted with my friends. I also gathered feedback on the UI of the application to try and make it easier to use, and used it to make changes - for example changing the colors of the 'Add to watchlist' and 'Not interested' to make their function more clear. Unfortunately, my lack of experience with CSS made this process more difficult, as I didn't always know how to make the changes that would make the application easier to use.
I also read up on ARIA, and set appriopriate roles and attributes in my application. I haven't yet been able to integrate it in the entire application - this would be a good next step.

# Research
I researched what the ACM Code of Ethics had to say about working together in a group, and how that code applied in our way of working, and in an educational
setting:  
[Ethics Research](https://github.com/FilmerApp/.github/blob/main/Research/Ethics.md)

I researched the optimal choice of Database and ORM framework for Filmer:  
[Database & ORM Research](https://github.com/FilmerApp/.github/blob/main/Research/Database%20and%20ORMs.md)

I researched cryptography and how it could be applied to increase the security of both Filmer and Eeventify:  
[Cryptography Research](https://github.com/FilmerApp/.github/blob/main/Research/Cryptography.md)

I am working on researching hypothetical scenarios for trying to create a business out of Eeventify.

I started researching agile working methods (research will be finished along with the Eeventify group):  
[Agile Research](https://github.com/FilmerApp/.github/blob/main/Research/Agile.md)

# Semester Reflection
This period started off quite badly for me - I got sick for almost a month, after which I moved, and all of this along with some other unfortunate personal situations meant that I wasn't able to make much progress the first half of the semester. After being sick, my main priority was catching up with the group project, which I think I did quite well. I worked hard familiarizing myself with the code written in my absence, and could get to work within a week. 

As for my personal project, I was still quite behind. Whenever this happens I tend to get very overwhelmed, and don't know where to start work in order to catch up, which makes the situation worse. However, while it wasn't as quickly as with the group project, I did manage to pick up speed, make a detailed planning for how I was going to finish the project, and ended up creating an applicatin that I'm personally quite happy with.

The biggest issue was asking for feedback. I don't know why, but when I have the feeling I'm behind i tend to 'shut down' which makes it incredibly hard to ask for help or even let others know how I'm doing. This is something I have struggled with for a long time, and I'm not sure what causes it (maybe I'm ashamed of my lack of progress). The longer I haven't spoken with my teachers, the harder it gets for me to approach them, even if I have caught up on my work. Obviously this doesn't really conform with the professional learning goal, but I am quite proud of the fact that I did manage to get back into the flow and finish my application - something I haven't always been able to do in the past. I managed to implement all the features I had planned, even though I had to learn to work with several technologies I had no experience with, something that can often make the process even more overwhelming.

I do also think I managed the group project relatively well: I think I carried my weight, and I think this is the area where I had the most growth when it comes to professionalism and interaction with stakeholders, especially because we had an interesting challenge in working both with our teachers and the groups from Finland, as well as setting our own development goals. Throughout the progress, we had a lot of dicussion about the features we wanted to deliver, not only within our group, but also with the groups from Finland. When we finished our collaboration with the other groups, we started creating our own frontend, working together with our teachers to decide what features were most important. Overall, I really enjoyed working together on this project; I think it helped that I stepped back into it relatively early, which means I had more interaction with our group project teachers, especially Bert. We also had a very pleasant working environment, our group worked very well together, and I'm personally very happy with the progress we made on Eeventify.

### Points of Improvement
The biggest - and most obvious - point of improvement is my communication with teachers. I really want to try to start off next semester well, and try to plan at least one feedback conversation every week from the start, which will make it much easier to keep up with what's expected of me. I do think I am getting better at things like this, but there's still a long way to go, and as long as I ensure to not fall behind too much, things won't get as overwhelming.


# Links
[Miscellaneous Design Diagrams](https://github.com/FilmerApp/.github/blob/main/profile/Design%20Documents.md)

[Ethics Research](https://github.com/FilmerApp/.github/blob/main/Research/Ethics.md)

[Database & ORM Research](https://github.com/FilmerApp/.github/blob/main/Research/Database%20and%20ORMs.md)

[Cryptography Research](https://github.com/FilmerApp/.github/blob/main/Research/Cryptography.md)
