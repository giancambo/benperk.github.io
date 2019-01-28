# Using NuGet to install your ORM into Visual Studio

I have spent a lot of time Working with NHibernate 3.0 and now am also giving some attention to the Entity Framework.  One of the road blocks for using NHibernate is the initial configuration requirements.  There are a significant number of actions which must be taken to get NHibernate working, where as, the Entity Framework almost works right out of the box.
There is a cool feature called “Package Manager Console” which allows you to install packages without having to download them from a website or source repository.  Instead you can install them from within Visual Studio directly into your Visual Studio solution.  This reduces the NHibernate configuration requires, a little, but even a little is ok.
To access the Package Manager Console, first create a Visual Studio project, and then select Tools => Library Package Manager => Package Manager Console as shown in Figure 1.

![How to access the Package Manager Console in Visual Studio][FIGURE1]
###### Figure 1, How to access the Package Manager Console in Visual Studio

Simply enter either:

+ install-package EntityFramework or
+ install-package NHibernate

and the binaries for those ORM’s will be added to the References in your project.  Then you can add your using directives for those ORM’s directly into your project as shown in Figure 2.

![Entity Framework and NHibernate directives][FIGURE2]
###### Figure 2, Entity Framework and NHibernate directives

I wrote another blog which discusses the implementation of both NHibernate and the Entity Framework [here][LINK1].

[FIGURE1]: ../images/2012/msdn-0061.png "Figure 1, How to access the Package Manager Console in Visual Studio"
[FIGURE2]: ../images/2012/msdn-0062.png "Figure 2, Entity Framework and NHibernate directives"

[LINK1]: ../2011/2011-10-nhibernate-and-entity-framework-essentials-using-a-model-first-approach.md
