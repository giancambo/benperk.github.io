# LINQ generated NHibernate and Entity Framework SQL (JOIN)

In continuation of a [previous][LINK1] blog which compared 2 simple SELECT statements between NHibernate and Entity Framework, I do the same with the joining of 2 tables together.

## NHibernate

Figure 1 shows the LINQ to NHibernate query which selects all the data from the PostForNHibernate table, joins with the BlogForNHibernate table using the foreign key where the BloggerName is Benjamin Perkins.

![LINQ to NHibernate JOIN with Lambda Expression][FIGURE1]
###### Figure 1, LINQ to NHibernate JOIN with Lambda Expression

Figure 2 shows the data result and the generated SQL from the source code shown in Figure 1.

![LINQ to NHibernate generated SQL – JOIN][FIGURE2]
###### Figure 2, LINQ to NHibernate generated SQL – JOIN

## Entity Framework

Figure 3 shows the LINQ to Entities query which selects all the data from the Post table, joins with the Blog table using the foreign key where the BloggerName is Benjamin Perkins.

![LINQ to Entities with Lambda expression][FIGURE3]
###### Figure 3, LINQ to Entities with Lambda expression

Figure 4 shows the data result and the generated SQL from the source code shown in Figure 3.

![LINQ to Entities generated SQL – JOIN][FIGURE4]
###### Figure 4, LINQ to Entities generated SQL – JOIN

## Conclusion

The SQL queries are almost exact excluding the placement of the search variable ‘Benjamin Perkins’.  I am almost convinced that the queries created by both NHibernate and Entity Framework are, at least at a simple level, the same.

I will perform some more tests with some Aggregates and commands like GROUP BY and ORDER BY and let you know the outcome.

[LINK1]: 2012-03-linq-generated-nhibernate-and-entity-framework-sql.md

[FIGURE1]: ../images/2012/msdn-0145.png "Figure 1, LINQ to NHibernate JOIN with Lambda Expression"
[FIGURE2]: ../images/2012/msdn-0146.png "Figure 2, LINQ to NHibernate generated SQL – JOIN"
[FIGURE3]: ../images/2012/msdn-0147.png "Figure 3, LINQ to Entities with Lambda expression"
[FIGURE4]: ../images/2012/msdn-0148.png "Figure 4, LINQ to Entities generated SQL – JOIN"
