# Miscellaneous Design Diagrams

## Database Diagram
The following diagram shows how my database is set up:

![Database diagram](https://github.com/FilmerApp/.github/blob/main/images/Database%20Diagram.png)

The recommendation and account services both contain their own, completely separate, database. The ID in the Users table in the account service corresponds to the UserId in the WathlistItems table in the recommendation service.
