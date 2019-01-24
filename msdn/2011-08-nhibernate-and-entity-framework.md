Prior to getting my job here at Microsoft, I was, and in many aspects still am passionate about NHibernate.  I discovered a particular liking to ORM’s in general.  I liked it so much that I wrote a book called “Working with NHibernate 3.0” which is available on Amazon or from the Wrox.com website.

What I like most about ORM concepts, perhaps is rooted withinn my own skill set.  I am a programmer and a designer at heart.  To be honest, I am not so good at writing complex SQL queries.  This is simply because on many of my past development teams there were always the programmers and the database people.  When I needed a complex query written, I would get the database team to do it.  With the creation of ORM technologies, I don’t need to know how to write SQL queries anymore, because the ORM converts my code into a “pretty optimized” SQL query for me. Now that I work at Microsoft I am shifting my focus to the ADO.NET Entity Framework.  Honestly, NHibernate has more capability today then the ADO.NET Entity Framework, but I ask myself "Is that a good thing or a bad thing?"

What I mean is that, for an individual or organization who wants to implement an ORM, if the chosen technology is NHibernate, it’s my opinion that the learning curve would be much greater.  For a manager that means more cost and more risk.  This is because of the many interfaces NHibernate provides for data querying.  For example, NHibernate provides the following query interfaces: IQuery (HQL), ICriteria, ICriteria with QueryOver, LINQ to NHibernate, LINQ to NHibernate with Lambda Expressions.  The first question is which one to use and why?  I cover this in my book… check it out [here][LINK1].

With EF, on the other hand, you are not confronted with this massive library of interfaces which “more or less” do the same thing.  Plus with EF you get a very nice GUI to create and design your data model.

I’ve only just begun my dive into the ADO.NET Entity Framework.  My plan is to take what I know from NHibernate and find the equivalent in EF and then  write/blog about the pros and cons of each.

[LINK1]: http://www.wrox.com/WileyCDA/WroxTitle/Working-with-NHibernate-3-0.productCd-1118112571.html
