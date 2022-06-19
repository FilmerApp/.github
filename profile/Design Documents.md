# Miscellaneous Design Diagrams

## Database Diagram
The following diagram shows how my database is set up:

![Database diagram](https://github.com/FilmerApp/.github/blob/main/images/Database%20Diagram.png)

The recommendation and account services both contain their own, completely separate, database. The ID in the Users table in the account service corresponds to the UserId in the WatchlistItems table in the recommendation service. Originally the Users table was designed to manage all logins, but since I have since switched to using Auth0, it is now only used to look up user IDs based on their email. If the email can't be found in the database, a new item will be made for that user along with a corresponding ID - this is possible because authentication has already been handled by Auth0.

One small point of improvement: in hindsight, it would have been better to link the Films and Genres tables with a coupling table, which would allow the algorithm to search IDs instead of strings, which would improve performance.
