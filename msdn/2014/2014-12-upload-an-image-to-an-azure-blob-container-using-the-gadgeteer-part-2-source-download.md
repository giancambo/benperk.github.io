# Upload an image to an Azure Blob container using the Gadgeteer (Part 2, source download)

If you have not already read the first part of this blog, please check it out Part 1 [here][LINK1].

## Uploaded the image to the Azure Blob container

This…..was…..hard….  The code wasn’t hard, but getting the header formatted into the correct format was a hurdle, using the Micro Framework.  It’s actually not possible to use the Micro Framework to create the header.  I’ll explain why, but first the 2 steps required to store the image in the Azure Blob container are:

+ Get and Set the UTC time on the Gadgeteer
+ Create the REST API for uploading to an Azure Blob

### Get and Set the UTC time on the Gadgeteer

I am certain there is an easier way or perhaps a more ‘network nerdy’ way to do this but I took the simple (or should I say easiest for me) route and used a WebAPI hosted on Azure Websites.  The WebAPI, shown in Listing 2, simply returns the date/time and then I used the result to set the time on the Gadgeteer, Listing 3, which exists in the ProgramStarted() method.

###### Listing 2, calling a WebAPI from a Gadgeteer using .NET Micro Framework

```
 DateTime setLocalTimeWebAPI()
 {
  try
  {
   System.Net.WebRequest request = System.Net.WebRequest
             .Create("https://??**??.azurewebsites.net/api/getdatetime/");
   System.Net.WebResponse response = request.GetResponse();
   System.IO.Stream dataStream = response.GetResponseStream();
   System.IO.StreamReader reader = new System.IO.StreamReader(dataStream);
   string responseFromServer = reader.ReadToEnd();
   reader.Close();
   response.Close();
   string string1find1 = "dateTime\":\"";
   string string1find2 = "\"";
   int begin = responseFromServer.IndexOf(string1find1) + string1find1.Length;
   int end = responseFromServer.IndexOf(string1find2, begin);
   string datetimeX = responseFromServer.Substring(begin, end - begin);
   DateTime time = new DateTime(Int32.Parse(datetimeX.Substring(6, 4)),
                       Int32.Parse(datetimeX.Substring(0, 2)),
                       Int32.Parse(datetimeX.Substring(3, 2)),
                       Int32.Parse(datetimeX.Substring(11, 2)),
                       Int32.Parse(datetimeX.Substring(14, 2)),
                       Int32.Parse(datetimeX.Substring(17, 2)));
   return time;
  }
  catch (Exception ex)
  {
   Debug.Print("WebAPI Exception: " + ex.Message);
  }
 }
 ```
 
I chose to use the IndexOf() method because of the missing JSON functionalities in the .NET Micro Framework. There are numerous places where this framework needs to simplified or enhanced, but until then, we developers have deadlines and we gotta get the code to work, right?

###### Listing 3, setting the date time on the Gadgeteer

```
DateTime time = new DateTime();
time = setLocalTimeWebAPI();
Microsoft.SPOT.Hardware.Utility.SetLocalTime(time);
```

Maybe when the Gadgeteer is ported to Visual Studio 2015 I can use Roslyn to optimize my code, until then, I will work with it.

### Create the REST API for uploading to an Azure Blob

This last part was the most challenging.  I used these articles as a model to help me through the process.

+ [Put Blob][LINK2]
+ [Azure Storage Samples][LINK3]

Building the code to create the header was pretty straight forward, the example code was very good.  There is no string.Replace() method in the .NET Microframework, so I replaced that with StringBuilder.

Using the code example found on codeplex I created the source in Listing 4, which format the header and calls the PUT REST API for my Azure Blob container.

###### Listing 4, Storing, saving, putting an image into an Azure Blob container using Gadgeteer, .NET Micro framework 4.3

```
public void PutBlob(String containerName, String blobName, byte[] blobContent)
{
 String requestMethod = "PUT";
 StringBuilder urlPath = new StringBuilder("{0}/{1}").Replace("{0}", containerName)
                                                     .Replace("{1}", blobName);
 String storageServiceVersion = "2009-09-19";
 DateTime currentDateTime = DateTime.UtcNow;
 String dateInRfc1123Format = currentDateTime.ToString("R"); //See NOTE below 
 Int32 blobLength = blobContent.Length;
 StringBuilder canonicalizedResource = new StringBuilder("/{0}/{1}")
                                       .Replace("{0}", Account)
                                       .Replace("{1}", urlPath.ToString());
 StringBuilder canonicalizedHeaders =
           new StringBuilder("x-ms-blob-type:{0} \nx-ms-date:{1} \nx-ms-version:{2}")
           .Replace("{0}", BlobType)
           .Replace("{1}", dateInRfc1123Format)
           .Replace("{2}", storageServiceVersion);
 StringBuilder stringToSign =
           new StringBuilder("{0} \n\n\n{1} \n\n\n\n\n\n\n\n\n{2} \n{3}")
           .Replace("{0}", requestMethod)
           .Replace("{1}", blobLength.ToString())
           .Replace("{2}", canonicalizedHeaders.ToString())
           .Replace("{3}", canonicalizedResource.ToString());
 string xMSBlobType = "x-ms-blob-type:BlockBlob";
 string dateNoHeader = currentDateTime.ToString("s");
 string xMSVersion = "x-ms-version:2009-09-19";
 string authorizationHeader = CreateAuthorizationHeaderWebAPI(requestMethod,
                              blobLength, xMSBlobType, xMSVersion,
                              canonicalizedResource.ToString(), dateNoHeader);
 Uri uri = new Uri(BlobEndPoint + urlPath.ToString());
 System.Net.HttpWebRequest request = (System.Net.HttpWebRequest)WebRequest.Create(uri);
 request.Method = requestMethod;
 request.Headers.Add("x-ms-blob-type", BlobType);
 request.Headers.Add("x-ms-date", dateInRfc1123Format);
 request.Headers.Add("x-ms-version", storageServiceVersion);
 request.Headers.Add("Authorization", authorizationHeader);
 request.ContentLength = blobLength;
 try
 {
  using (Stream requestStream = request.GetRequestStream())
  {
   requestStream.Write(blobContent, 0, blobLength);
  }
  using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
  {
   Debug.Print("HttpWebResponse.StatusCode: " + response.StatusCode.ToString());
  }
 }
 catch (WebException ex)
 {
  Debug.Print("An error occured. Status code:" +
             ((HttpWebResponse)ex.Response).StatusCode);
 }
}
```

Firstly, the date / time setting is a very important part of the Authorization Header and found that it can be about 30 seconds different than the time on the Azure server.  That being said, the format of the date time, when I originally formatted it was “s”, which includes a space.  The standard format can be found here.  And after intense debugging, I was finally able to determine that the WebAPI from the Gadgeteer does not like spaces in the parameters, and because the date time format I had chosen, or was actually default, was causing the hashed value to come back invalid.

The solution was to change the format of the date time into one which had no spaces “R”, and then the WebAPI converted it back to “s” and hashed the header. Listing 5 shows the CreateAuthorizationHeaderWebAPI() method used to call the WebAPI that hashes the Authorization header.  This method is called from the code in Listing 4.

You might be asking yourself, why are you calling a WebAPI to hash you header instead of using the System.Security.Cryptography.HMACSHA256 method?  The answer is because it doesn’t exist in the .NET Microframework and is why I said earlier that it was not possible to call an Azure Storage REST API using the .NET Microframework.

###### Listing 5, Using a WebAPI to hash an authorization header on Gadgeteer

```
private static string CreateAuthorizationHeaderWebAPI(string requestMethod,
                       int blobLength, string xMSBlobType, string xMSVersion,
                       string canonicalizedResource, string now)
{
 try
 {
  string hashedValue = string.Empty;
  string SharedKeyAuthorizationScheme = "SharedKey";
  string queryString = "requestMethod=" + requestMethod + "&blobLength=" +
                       blobLength.ToString() + "&xMSBlobType=" + xMSBlobType +
                       "&xMSVersion=" + xMSVersion + "&canonicalizedResource=" +
                       canonicalizedResource + "&now=" + now;
  Uri uri = new Uri("https://*?*.azurewebsites.net/api/HMACSHA256?" + queryString);
  System.Net.HttpWebRequest request =
                      (System.Net.HttpWebRequest)System.Net.WebRequest.Create(uri);
  System.Net.HttpWebResponse response =
                      (System.Net.HttpWebResponse)request.GetResponse();
  System.IO.Stream dataStream = response.GetResponseStream();
  System.IO.StreamReader reader = new System.IO.StreamReader(dataStream);
  string responseFromServer = reader.ReadToEnd();
  reader.Close();
  response.Close();
  string string2find1 = "HashedValue\":\"";
  string string2find2 = "\"";
  int start = responseFromServer.IndexOf(string2find1) + string2find1.Length;
  int stop = responseFromServer.IndexOf(string2find2, start);
  hashedValue = responseFromServer.Substring(start, stop - start);
  StringBuilder authorizationHeader = new StringBuilder("{0} {1}:{2}")
                                      .Replace("{0}", SharedKeyAuthorizationScheme)
                                      .Replace("{1}", "put")
                                      .Replace("{2}", hashedValue);
  return authorizationHeader.ToString();
 }
 catch (System.Net.WebException ex)
 {
  Debug.Print("Exception: " + ex.Message);
  return null;
 }
}
```

Again, the result of the WebAPI is JSON and I could not find any library for JSON for use with the .NET Microframework, so I derived the above code.

The code contained in the WebAPI is nothing more than the original code found in the example on codeplex, similar to that shown in Listing 6.

###### Listing 6, Hash logic for the PUT header for an Azure Blog container

```
private string CreateAuthorizationHeader(string canonicalizedstring)
{
 string signature = string.Empty;
 using (HashAlgorithm hashSHA256 = new HashAlgorithm(HashAlgorithmType.SHA256))
 {
  Byte[] dataToHmac = System.Text.Encoding.UTF8.GetBytes(canonicalizedstring);
  signature = Convert.ToBase64String(hashSHA256.ComputeHash(dataToHmac));
 }
 StringBuilder authorizationHeader = new StringBuilder("{0} {1}:{2}")
                            .Replace("{0}", SharedKeyAuthorizationScheme)
                            .Replace("{1}", Account)
                            .Replace("{2}", signature);
 return authorizationHeader.ToString();
}
```

Lastly, the code I used to consume the PutBlob() method, Listing 4, is shown in Listing 7.  Notice in Listing 7 that you need to place you Azure Blob Storage name ‘put001’ and the container name ‘blob’ into this method, remember creating them long ago in Figure 1 and Figure 2?  This is used, as shown in Listing 4, to create the REST API and Authorization Header.

###### Listing 7, how to store an image in an Azure Blob container using Gadgeteer and .NET Microframework

```
void insertImageintoAzureBlob(GT.Picture picture, string fileName)
{
AzureBlob storage = new AzureBlob()
 {
  Account = "put001", //"put001" is your storage account name
  BlobEndPoint = "https://***.blob.core.windows.net/", //***="put001"
  Key = "YOUR BLOB KEY"
 };
 if (ethernetJ11D.IsNetworkUp)
 {
  try
  {
   //"blob" is the container name
   storage.PutBlob("blob", fileName, picture.PictureData, error);
  }
  catch (Exception ex)
  {
   Debug.Print("EXCEPTION: " + ex.Message);
  }
 }
 else
 {
  Debug.Print("NO NETWORK CONNECTION");
 }
}
```

The Key property of the AzureBlob class is what the REST API uses to hash and validate that you, the client, have access to PUT an image into the Azure Blob container.  You can find this by clicking the KEYS tile within the Azure Management console, similar to that shown in Figure 6.

![get the access key for your Azure Blob container][FIGURE1]
###### Figure 6, get the access key for your Azure Blob container

And lastly, notice that the parameter for the insertImageintoAzureBlob() method is GT.Picture, which I referred to earlier.  The GT.Picture property PictureData provides a byte[] type which contains the picture and this just so happens to be the same format required for the RequestStream.Write() method used in Listing 4 to send the image to the REST API.  One of the places where, it just worked out good, thanks, I needed that.

## Conclusion

That’s it.  I hope you find this blog helpful, I could not have been successful without everyone else in the Gadgeteer community sharing and writing about their experiences.  Therefore, I am sharing and writing about mine too in hopes it helps move this cool Internet of Things era forward.


[FIGURE1]: ../images/2014/msdn-0482.png "Figure 6, get the access key for your Azure Blob container"

[LINK1]: 2014-12-upload-an-image-to-an-azure-blob-container-using-the-gadgeteer-part-1.md
[LINK2]: http://msdn.microsoft.com/en-us/library/azure/dd179451.aspx
[LINK3]: http://azurestoragesamples.codeplex.com/
