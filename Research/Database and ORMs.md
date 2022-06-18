# Databases, Database Management Systems, and Object-Relational Mapping Frameworks
The interaction with the database is one of the fundamental functions of many applications, and programmers are likely to spend a lot of time on that interaction. Not only that: because the database serves as a foundation for the application, it has a major impact on its performance and how well it works. This means it’s vital to make informed decisions surrounding your database.

SQL databases are the most popular form of database, there are many database management systems (DBMS) to - like the name implies - manage those databases, and all these choices can be overwhelming when you first start to look into them.

Object-Relational Mapping is a technique that allows a user to manipulate data from a database by way of objects. An Object-Relational Mapping Framework (referred to as ORM) is a library that implements this technique - it ensures that the programmer will no longer need to write direct SQL queries, but instead allows them to interact with the database using the same programming language as the rest of the application.

Because of the added layers of abstraction and increased automation an ORM offers, they can be a big time-saver; a programmer needs to spend less of his time writing SQL queries, and can instead work with clear statements that make the program easier to read and debug.

# Why does it matter?

There are two ways to look at ORMs and databases (and their DBMS); from the point of view of the developer, and from the point of view of the application itself.

ORMs offer a lot of advantages to developers; not only is it easier to use one than to keep writing error-prone SQL queries, but most ORMs allow you to directly perform complex operations on the database, which ensures you don’t need to manually read data from the database and create objects from them, something that increases the change of bugs and complicates debugging. When using an ORM it doesn’t matter quite as much how easy the DBMS is to use, but it’s ease of install is still an important factor.

When it comes to the application, one of the major concerns is performance: while ORMs simplify the programming process, they do carry a risk of being slower than well-written SQL. Especially when working with complex operations on a large database, a small performance difference can have a large impact. And while the difference between DBMS is often small, that difference does exit: some DBMS might be slightly faster than their counterparts when it comes to read-heavy applications or write-heavy databases.

All of this means we need an ORM and DBMS that make the development process easier, without negatively impacting performance too much. Unfortunately, there is no objective best choice - instead it will depend on the needs of the application itself. This is why I will try to find the best option for my own application, Filmer.

# The Database

The first important choice is that of a database, and database management system. Much of this choice comes down to personal preference, and there are too many DBMS to compare in-depth here. I did read up on them, and in the end have chosen Microsoft SQL Server. My most important reasons are as follows:

- The installation is easy, especially in a .NET environment
- Usage is relatively simpel (as opposed to ie PostgreSQL), and there is no need to learn additional query languages
- It is fast, especially in databases where a lot of data has to be read, like with Filmer
- I have experience using it

One of SQL Server’s big downsides is the fact that standard licenses aren’t free, but in my case I can make use of a student license, which will serve me well for the scope of this project. However, if the application were to be released on a larger scale, usage of this DBMS would bring additonal costs with it.

# Subjective Considerations

Many of the considerations when it comes to ORMs come down to personal preference instead of objective facts; how much ease of use is worth is something that will differ by developer.

For many projects, meeting deadlines and completing work quickly will be very valuable, and the ease of use of an ORM will help greatly with these, as well as making testing and bugfixing easier, which will ensure a more stable application.

# Research Details

The most of objective way of establishing which ORM would be fastest for my specific application would be to develop all of the application, create a version with every possible ORM, execute the database-related functions, and measure performance.

Obviously, this is not realistic: because ORMs often differ wildly in their implementation, I would need to rewrite large parts of my code for every test, which would be a massive amount of work. Instead, we need to set up a test that is as representative of my application’s needs as possible, and use that to measure ORM speed.

First of all, I need to establish what interactions Filmer will have with the database. The most important are as follows:

1. Retrieving all films from the database - a large read operations, that requires reading data from multiple tables and combining it into objects.
2. Determine a user’s favorite genres by reading their watchlist - a relative complex read operation that will require reading data from multiple tables.
3. Saving a film on a user’s watchlist - a small and simple write operation.
4. Adjusting a film on a user’s watchlist to indicate it has been watched, liked, or disliked - a simple and small update operation.
5. Reading a user’s data when logging in - a simple read operation.
6. Periodically updating the general film database based on an external database - a large update operation that will potentially add many new films, and details of every movie could potentially be edited, for example when their rating has changed.

There are a few other potential operations, like registering new users, but those are small and infrequent enough to not have a large impact on the application’s speed.

There are a couple of operations in this list that require reading of writing a lot of date, and will have a large impact on performance. The biggest of those are 1, 2, and 6. 1 and 2 are both read operations, and 6 is a write operation. The speed of the latter is less important however, because it occurs very infrequently: once a day at most.

1 and 2 are used relatively often - every time a new set of recommendations has to be generated for a user. Preferably, the application shouldn’t lag every time this happens, so performance is essential.

While 3 and 4 will occur relatively often, it will at most be once every 3-5 seconds. Because both cases only concern the manipulating of a single row in a table, the total operation time will be extremely low, and small speed differences between ORMs will not have a noticeably effect. 

5 is a simple operation as well, and will happen infrequently, especially when a user chooses to stay logged in for an extended period of time.

From this, we can conclude that the most important factor for our ORM is how fast it can perform a large read-operation, not only in a single table, but also for multiple linked tables. The speed of write operations on single rows is important enough to test, but not a priority.

# Methodology

To perform the experiment, we need to set up a simple data model. The exact details don’t matter, so I suggest some simple objects: a Student, a Class, and a Grade. Students have Classes, and can have a Grade for every Class.

Next, we will need to create a database and populate it with a large amount of data - the exact data can be random, but we need enough data to be able to detect a noticeable difference between the ORMs we test, so let’s fill all tables with 200.000 rows of data.

Now we can set up our ORMs, in the same way we would for any application. Even though setting these up still requires some effort, it is much easier now that we have a simple framework for our tests, and only need to perform a few operations. We do need to make a selection of ORMs we want to test, and I would pick some of the more popular ORMs: Entity Framework Core, Entity Framework 6, Dapper, Linq to DB, and NHibernate. How complex and abstract these are differs: Dapper is a ‘micro ORM’ that still requires you to write SQL statements, Linq to DB has some layer added on top of that, and Entity Framework has a much larger amount of features and abstraction. The difference between these will be interesting to measure.

We will also need something to easily perform benchmarks and measure their results: Microsoft Crank is a good option.

Now we can perform our first test: reading a large amount of data. We make every ORM read all Students and their Classes and save this data in a list, and measure the time it took to perform this operation. The second test is an individual write-operation, where a new Student is written to the database, along with a few Classes. Because a single write-operation will be incredibly quick, even on a large database, we will perform this test 5000 times to be able to measure a clear difference in speed.

After these tests are performed, the data can be compared. Unfortunately, I wasn’t able to find the time to set up all of these test scenarios myself, but I have collected some results from similar experiments.

![ORM Comparison 1](https://github.com/FilmerApp/.github/blob/main/images/ORM%20Comparison%201.png)

Source: skepee ([https://github.com/skepee/Orm-Benchmark](https://github.com/skepee/Orm-Benchmark))

![ORM Comparison 2](https://github.com/FilmerApp/.github/blob/main/images/ORM%20Comparison%202.png)

Source: Frans Bouma ([https://github.com/FransBouma/RawDataAccessBencher/blob/master/Results/20220113_net6.txt](https://github.com/FransBouma/RawDataAccessBencher/blob/master/Results/20220113_net6.txt))

Comparison between Entity Framework 6 and Entity Framework Core, Chad Golden ([https://chadgolden.com/blog/comparing-performance-of-ef6-to-ef-core-3](https://chadgolden.com/blog/comparing-performance-of-ef6-to-ef-core-3))

# Conclusion

In the end, for my own application, I have chosen to use Entity Framework Core. While I couldn’t perform the exact tests I would have liked, based on other people’s benchmarks this framework is relatively fast when it comes to large read-operations, and very fast for small write-operations, which fits my application’s requirements well. Entity Framework Core seems to be faster than Entity Framework 6 across the board in most results.

While there are definitely faster choices, those do not have the flexibility and options that Entity Framework offers, something that will greatly streamline my development process. Also, Entity Framework integrates easily in a .NET environment, and it’s the ORM that I have the most experience using.

A last consideration: it is obviously possible to combine different ORMs. In a later project I could choose to use Entity Framework for operations where performance is not important, and switch to something like Dapper for operations where it is.
