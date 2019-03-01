# ASP.NET WebAPI results in a 405 Method Not Allowed HTTP response

I was working on an IoT project where my device was calling a WebAPI.  I had two methods, one had the signature shown in Listing 1 and the other in Listing 2.

***UPDATE*** 6-JUN-2016, check [here][LINK1] for some exception handling tips.

###### Listing 1, GET employees

```
[Route("api/TC/{custId}")]
public IQueryable<EmployeeDTO> GetEmployees(string custId)
{
 var db = new fhEntities();
 return from emp in db.EMPLOYEEs
 select new EmployeeDTO()
{
FIRSTNAME = emp.FIRSTNAME,
LASTNAME = emp.LASTNAME
};
}
```

The code for the GetEmployees method, is really a test method.  It returns a list of all employees using an Entity Framework query and is accessible on my Azure Web App at, for example: “??**??.azurewebsites.net/api/TC/2”.

###### Listing 2, Clock In or Out 

```
[Route("api/TC/{custId}/{rfId}/{clockedDate}/{clockedTime}")]
public IHttpActionResult ClockInOut(string custId, string rfId, string date, string time)
```

I created a test console application for running through all my use cases and found that when I executed the code in Listing 1, everything worked ok, however executing the code in Listing 2 did not.  The call to the ClockInOut() method resulted in a 405.

Using Fiddler I looked at the request to the method shown in Listing 1 and saw the result shown in Figure 1.

![WebAPI GET request example][FIGURE1]
###### Figure 1, WebAPI GET request example

And executing the request to CheckInOut() resulted in a 405 Method not allowed, as shown in Figure 2.

![WebAPI GET request results in 405 method not allowed][FIGURE2]
###### Figure 2, WebAPI GET request results in 405 method not allowed

How could that be?  Both are GET requests.

The answer is that as per naming conventions, because I added the work ‘Get’ to the beginning of the method as shown in Listing 1, the WebAPI logic interpreted it as a GET request.  However, because there was no VERB definition on my method shown in Listing 2, I get this 405 method not allowed response when I attempt to call it.

The solution is to rename my method so that it complies with the naming standards.  Therefore I renamed my ClockInOut() to GetClockInOut(), as shown in Listing 3, and all worked as expected.

###### Listing 3, How to fix/resolve a 405 from a WebAPI call

```
public IHttpActionResult GetClockInOut(string custId, string rfId, string date, string time)
```

Additionally, if you wanted to leave the method name unchanged, you can add the [HttpGet] annotation above the definition of the method as shown in Listing 4.

###### Listing 4, using annotations for verb definition with WebAPI

```
[HttpGet]
[Route("api/TC/{custId}/{rfId}/{clockedDate}/{clockedTime}")]
public IHttpActionResult ClockInOut(string custId, string rfId, string date, string time)
```

I write this article because when I initially had this issue, after searching for some explanation on it was not easily findable.  However, after researching it makes sense and I write this blog to help people possibly find the solution faster and get back to doing some development and project progress.  Let’s code!

Figure 3 is a picture of the time clock I built using the Gadgeteer.  At the moment it does use the Ethernet J11D module but will be enhancing it to use the WiFi RS21 Module with Antenna.

![an example of a time clock using the Gadgeteer][FIGURE3]
###### Figure 3, an example of a time clock using the Gadgeteer

[FIGURE1]: ../images/2015/msdn-0529.png "Figure 1, WebAPI GET request example"
[FIGURE2]: ../images/2015/msdn-0530.png "Figure 2, WebAPI GET request results in 405 method not allowed"
[FIGURE3]: ../images/2015/msdn-0531.png "Figure 3, an example of a time clock using the Gadgeteer"

[LINK1]: http://www.asp.net/web-api/overview/error-handling/exception-handling
