# Using Entity Framework and Web API, I get a 500 Internal Server Error or a Hang

I was working on some code that used Entity Framework and an ASP.NET WebAPI as the interface for accessing the data.  With my experience so far, when I publish my WebAPI and then access it, the result is a JSON formatted file that I can open or save via a message popup similar to that shown in Figure 1.

***UPDATE*** 6-JUN-2016, check [here][lINK1] for some exception handling tips.

![downloading a JSON file result from an ASP.NET WebAPI][FIGURE1]
###### Figure 1, downloading a JSON file result from an ASP.NET WebAPI

However, in this case I was either getting a 500 error as shown in Figure 2 or when I clicked on the Open button it would just hang or result in the error.

![500 Internal Server Error using Entity Framework and ASP.NET WebAPI][FIGURE2]
###### Figure 2, 500 Internal Server Error using Entity Framework and ASP.NET WebAPI

The ultimate solution came to me when added this line of code, shown in Listing 1, to the App_Start/WebApiConfig.cs file:

###### Listing 1, enable JSON reference loop handling formatters

```
config.Formatters.JsonFormatter.SupportedMediaTypes.Add(
        new MediaTypeHeaderValue("text/html"));
GlobalConfiguration.Configuration.Formatters.JsonFormatter
  .SerializerSettings.ReferenceLoopHandling =
Newtonsoft.Json.ReferenceLoopHandling.Ignore;
```

Once added, the exception was rendered to the browser instead of the HTTP status code.

Long story short, I was doing 2 things wrong.

Firstly, I was attempting to project my query result onto a mapped entity.  In short, this means that I was creating an instance of my dbContext DbSet class and attempting to map my result into that class instead of creating a DTO and I found some good information about this here.

Lastly, I was returning an IHttpActionResult instead of an IQueryable<T> result.  There are probably logical explanations, but I really just wanted to get my program to work.

For example, this did not work for me, Listing 2:

###### Listing 2, WebAPI and Entity Framework does not work for me

```
 public IHttpActionResult GetEmployees(string custId)
  {
    using (var db = new homeEntities())
    {
      var employee = from emp in db.EMPLOYEEs
      select new EmployeeDTO()
      {
        FIRSTNAME = emp.FIRSTNAME,
        LASTNAME = emp.LASTNAME
      };
      return Ok(employee);
     }
   }
```

While this one did, see Listing 3:

###### Listing 3, WebAPI and Entity Framework does  work for me

```
public IQueryable<EmployeeDTO> GetEmployees(string custId)
{
  var db = new homeEntities();
  return from emp in db.EMPLOYEEs
  select new EmployeeDTO()
  {
    FIRSTNAME = emp.FIRSTNAME,
    LASTNAME = emp.LASTNAME
  };
 }
```

Then via a C# Console application using this code, shown in Listing 4, I consumed the ASP.NET WebAPI and moved along the project.

###### Listing 4, Consuming a WebAPI that uses Entitly Framework with a C# console application, no problem

```
string URL = "https://???**???.azurewebsites.net/api/GetEmployees/2";
System.Net.WebRequest request = System.Net.WebRequest.Create(URL);
System.Net.WebResponse response = request.GetResponse();
System.IO.Stream dataStream = response.GetResponseStream();
System.IO.StreamReader reader = new System.IO.StreamReader(dataStream);
string responseFromServer = reader.ReadToEnd();
reader.Close();
response.Close();
Console.WriteLine("Response from WebAPI: " + responseFromServer);
Console.WriteLine();
Console.WriteLine();
```

[FIGURE1]: ../images/2015/msdn-0527.png "Figure 1, downloading a JSON file result from an ASP.NET WebAPI"
[FIGURE2]: ../images/2015/msdn-0528.png "Figure 2, 500 Internal Server Error using Entity Framework and ASP.NET WebAPI"

[LINK1]: http://www.asp.net/web-api/overview/error-handling/exception-handling
