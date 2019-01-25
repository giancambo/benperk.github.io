# NHibernate and Entity Framework essentials using a Model First approach

Object Relational Mapping technologies have been around for many years.  Hibernate was one of the first, if not the first, ORM library which targeted the Java platform.  Today, there are also NHibernate, which is a port of Hibernate to C# and the ADO.NET Entity Framework.  Being a huge fan of the concept I am passionate about finding and sharing best practices for the implementation of this technique.
There exist a number of implementation techniques within both NHibernate and the ADO.NET Entity Framework.  There is the Database First approach, which is used when the database already exists and the IT organization wants to apply an ORM on top of it.  Another possibility, which is the method discussed in this article is the Model First approach. 
Building a data model at the beginning of a project is common practice.  However, the activities which occur afterwards have serious implication on future maintenance and support activities.  Prior to ORM, the next steps would be to create the database, build the business logic layer and then the presentation layer, often all 3 in parallel.  What this means is that the business logic and in many cases, the presentation layer becomes reliant on and attached to the database structure and the database management system.  Therefore, all future modifications to the program begin with and are restricted by the chosen database management system and the corresponding database structure.
An alternative approach to directly building the database after the data models’ design is to instead begin the implementation of an ORM, such as NHibernate or ADO.NET Entity Framework.  The difference and benefit is that maintenance and enhancements will be driven from an object-oriented class structure and corresponding mapping files instead of a database structure.  That is significant because the relationships built between your classes in the code will match the relationships between your database tables.  It means that your database primary and foreign keys are based upon the relationships built into your code; you get a data structure which matches the code and that is a big win.  But be aware that the focus significantly shifts your approach from a database driven design to an object model driven design, and that is a substantial shift in culture and skillset.
There is some discussion about which ORM is the best, mostly when comparing NHibernate and the ADO.NET Entity Framework.  In this article I will show how to implement a simple Model First program using both NHibernate and the ADO.NET Entity Framework from within Visual Studio.  This article is not intended to prove which one is best, it is intended only to show how to implement this approach.  I will leave the decision towards which technology to implement to the organizations choosing to use them.

## Creating the data model

There are a number of ways to create the data model.  For this example I used an empty ADO.NET Entity Data Model template which provides a nice graphical user interface for adding properties and relationships.  There are a number of other tools such as Argo UML, a Visual Studio Modeling Project, ERWIN, MySQL Workbench or any of the many other tools to create a data model with.  Ultimately, I created a data model which resembled the following shown in Figure 1.

![a simple data model][FIGURE1]
###### Figure 1, a simple data model

## Creating the entities

When using the ADO.NET Entity Data Model template to build the data model, all the source code required for entity storage and their relationships are generated for you.  However, using NHibernate, the developer needs to create these entities and relationships primarily by hand.  As shown in Listing 1, the mapping by code approach which is new in version 3.2 is implemented.  Mapping the entities by code removes the requirement for the HBM XML mapping files.

![creating and mapping an entity in NHibernate 3.2][FIGURE2]
###### Listing 1, creating and mapping an entity in NHibernate 3.2

## Create the database structure

To implement the data model using the ADO.NET Entity Data Model, right-click within the design layout of the .edmx file and select Generate Database from Model….  Then walk through the wizard which asks which Data Connection to use or to create a new connection or database.  The wizard creates the SQL needed to build the already designed data model shown in Figure 1 and adds the connection string to the app.config or web.config file.  Listing 2 shows an example of the Customer table with the relationship to the Orders table.

![ADO.NET Entity Framework generated SQL][FIGURE3]
###### Listing 2, ADO.NET Entity Framework generated SQL

You need to then either manually execute the SQL on the database from, for example the SQL Server Management Studio or click the Execute DDL check box from within the wizard.  Once executed, you should have a database which resembles Figure 2.

![database structure created by ADO.NET Enetity Framework generated SQL][FIGURE4]
###### Figure 2, database structure created by ADO.NET Enetity Framework generated SQL

NHibernate does not show the developer the SQL required to build your data structure.  NHibernate provides a class called SchemaExport which contains the method Create().  You associate the entities to an NHibernate configuration object and pass that configuration to the constructor of the SchemaExport class when you want to build your data structure, as shown in Listing 3.

![create the database structure using NHibernate][FIGURE5]
###### Listing 3, create the database structure using NHibernate

## Retrieving the data stored in the database structure

Selecting the data from the data source using LINQ to Entities is achieved by the code shown in Listing 4.  The first step is to create an ObjectContext called dataContext which is the ADO.NET Entity Framework class that facilitates the interaction with the entity data.  Next is the LINQ query which uses the CustomerSet that returns an ObjectSet of type Customer.  The results are then looped through using embedded foreach loops.

![How to retrieve data from a data source using ADO.NET Entity Framework][FIGURE6]
###### Listing 4, How to retrieve data from a data source using ADO.NET Entity Framework

The code shown in Listing 5, shows how to achieve the same result from using LINQ to NHibernate.  The first step is to create the session object which is the NHibernate class that facilitates the interaction with the entity data.  Then create the transaction object which can be rolled back if an error occurs.  The LINQ to NHibernate capabilities are utilized by accessing the Query<T>() method of the session object.  The LINQ query returns an IQuerable<out T> which is looped through using embedded foreach loops.

![How to retrieve data from a datasource using NHibernate][FIGURE7]
###### Listing 5, How to retrieve data from a data source using ADO.NET Entity Framework

Both LINQ to Entities and LINQ to NHibernate result in a display as shown in Figure 3.

![the result of using LINQ to Entities or LINQ to NHibernate][FIGURE8]
###### Figure 3, the result of using LINQ to Entities or LINQ to NHibernate

## Conclusion

Both NHibernate and the ADO.NET Entity Framework provide the capabilities required to implement an ORM using the Model First approach.   The ADO.NET Entity Framework creates the classes defined in the data model for you, NHibernate does not.  Nonetheless, from a developer point of view, creating the entities and their relationships by hand does provide greater insight into the detailed aspects of the program.  The dynamically generated EF source code is big and many of the attributes and methods are not intuitively useful at first glance.   Creating these entities and the relationships between them result in having a code base containing only what is required.  The ADO.NET Entity Framework also creates a repository like framework which is a wrapper around the specific CRUD activities of EF.  There is no such functionality in NHibernate.  The creation of a repository or data access control (DAC) is something which needs to be designed, coded and tested.
Having used NHibernate extensively and even written a book about it, a common question is regarding which interface should be used.  NHibernate provides a number of them for the retrieval of data.  The answer is not one easily answered but the suggestion is to pick one and stick with it throughout the programs lifetime.  I believe the same applies when deciding between NHibernate and the ADO.NET Entity Framework.  Both provide ample capabilities to build most of today’s programs, you just need to decide which one fits into your companys’ culture and skill set, and stick with it.

[FIGURE1]: ../images/2011/msdn-0010.png "Figure 1, a simple data model"
[FIGURE2]: ../images/2011/msdn-0011.png "Listing 1, creating and mapping an entity in NHibernate 3.2"
[FIGURE3]: ../images/2011/msdn-0012.png "Listing 2, ADO.NET Entity Framework generated SQL"
[FIGURE4]: ../images/2011/msdn-0013.png "Figure 4, database structure created by ADO.NET Enetity Framework generated SQL"
[FIGURE5]: ../images/2011/msdn-0014.png "Listing 3, create the database structure using NHibernate"
[FIGURE6]: ../images/2011/msdn-0015.png "Listing 4, How to retrieve data from a data source using ADO.NET Entity Framework"
[FIGURE7]: ../images/2011/msdn-0016.png "Listing 5, How to retrieve data from a datasource using NHibernate"
[FIGURE8]: ../images/2011/msdn-0017.png "Figure 3, the result of using LINQ to Entities or LINQ to NHibernate"
