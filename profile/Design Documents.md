# Miscellaneous Design Diagrams

## Database Diagram
The following diagram shows how my database is set up:

![Database diagram](https://github.com/FilmerApp/.github/blob/main/images/Database%20Diagram.png)

The recommendation and account services both contain their own, completely separate, database. The ID in the Users table in the account service corresponds to the UserId in the WatchlistItems table in the recommendation service. Originally the Users table was designed to manage all logins, but since I have since switched to using Auth0, it is now only used to look up user IDs based on their email. If the email can't be found in the database, a new item will be made for that user along with a corresponding ID - this is possible because authentication has already been handled by Auth0.

One small point of improvement: in hindsight, it would have been better to link the Films and Genres tables with a coupling table, which would allow the algorithm to search IDs instead of strings, which would improve performance.

## C4 Model
This first diagram shows the way a Filmer user would interact with the application. There isn't much happening here, since I don't use a lot of external libraries or technologies. Ideally, the app would get its data from an external database (like that of IMDB) but I haven't been able to set that up yet.

![C4 Model - Context](https://github.com/FilmerApp/.github/blob/main/images/C4%20Model%20-%20Context.png)

This next diagram shows the two containers I have set up, and how the React application interacts with them. Like mentioned before, the Account Service was originally set up to handle all registration and editing of accounts, as well as logging in, but most of that functionality has been replaced by Auth0.

![C4 Model - Container](https://github.com/FilmerApp/.github/blob/main/images/C4%20Model%20-%20Container.png)

This final diagram zooms in on the Recommendation Service, which consists of two APIs: one for supplying the user with recommendations and one for viewing the user's watchlist, as well as allowing them to indicate films on their list, and whether or not they liked them. The recommendation API uses an algorithm, which gets a list of all films, as well as the user's watchlist, from the database. Both APIs share the same database, due to the interlinked nature of the data they use. Originally I had planned to split these APIs into two different containers, but that proved challenging when trying to cross-reference the necessary data.

![C4 Model - Component](https://github.com/FilmerApp/.github/blob/main/images/C4%20Model%20-%20Component.png)
