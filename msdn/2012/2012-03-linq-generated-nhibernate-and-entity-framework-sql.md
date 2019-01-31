# LINQ generated NHibernate and Entity Framework SQL

One of my favorite aspects of ORMs is that I no longer have to write SQL.  I like this because I was never really good at it once it exceeded the joining of 2 tables.  The really complicated queries I left with the SQL people or a DBA.  Nonetheless, I still wanted to look at the SQL queries that my selected ORM was generating from my IQuery, ICriteria or LINQ interface.
Here is an overview of using LINQ to NHibernate and LINQ to Entities on the same database and their corresponding generated queries.

## Entity Framework

Figure 1 shows my LINQ to Entities query which selects a single row from the Blog table where the Id is 1.  I have included this code in a sample console application.  You see that the query is accessible by converting the results variable to a string and then simply writing it to the Console.

![LINQ to Entities with Lambda Expression][FIGURE1]
###### Figure 1, LINQ to Entities with Lambda Expression

Figure 2 shows the generated SQL.  I like the format of it, it does not require any reformat to get it into a readable state.

![LINQ to Entities generated SQL][FIGURE2]
###### Figure 2, LINQ to Entities generated SQL

## NHibernate

Figure 3 shows my LINQ to NHibernate query which also selects a single row from the BlogForNHibernate table where the Id is 1.

![LINQ to NHibernate with Lambda Expression][FIGURE3]
###### Figure 3, LINQ to NHibernate with Lambda Expression

NHibernate logs the generated SQL to the console by default, so there really isn’t anything you need to do to see it if your running your code from a console application.  I used the built in BasicFormatter on the SQL query to format it and make it look better, because without it the query is written as a single line.  Figure 4 shows the code written after formatting and Figure 5 shows how it is written without formatting.  It’s not really a problem with these simple queries, but as the queries get more complex you may want to see them in a more readable format.

![LINQ to NHibernate formatted generated SQL][FIGURE4]
###### Figure 4, LINQ to NHibernate formatted generated SQL

![LINQ to NHibernate unformatted generated SQL][FIGURE5]
###### Figure 5, LINQ to NHibernate unformatted generated SQL

## Conclusion

First, the only difference I see is the location of the column and search parameter in the WHERE clause.  In the Entity Framework query there is WHERE 1 = Id while with NHibernate there is WHERE Id = 1.  Like I said I am not a DBA so I do not know if this makes a difference, but I will go out on a limb and say it does not.

Secondly, like I mentioned in some other blogs, Entity Framework has the benefit of working right out of the box, there was limited configuration required to get my DbContexts and DbSets up and running.  This is not the case with NHibernate, unfortunately.  Using NuGet to install the NHibernate package, which I wrote about [here][LINK1], does reduce the effort a little.  However, I am still required to create the configuration, the mappings and the SessionFactory. 

Lastly, there is not much difference between the generated queries.

[LINK1]: 2012-03-using-nuget-to-install-your-orm-into-visual-studio.md

[FIGURE1]: ../images/2012/msdn-0110.png "Figure 1, LINQ to Entities with Lambda Expression"
[FIGURE2]: ../images/2012/msdn-0111.png "Figure 2, LINQ to Entities generated SQL"
[FIGURE3]: ../images/2012/msdn-0112.png "Figure 3, LINQ to NHibernate with Lambda Expression"
[FIGURE4]: ../images/2012/msdn-0113.png "Figure 4, LINQ to NHibernate formatted generated SQL"
[FIGURE5]: ../images/2012/msdn-0114.png "Figure 5, LINQ to NHibernate unformatted generated SQL"
