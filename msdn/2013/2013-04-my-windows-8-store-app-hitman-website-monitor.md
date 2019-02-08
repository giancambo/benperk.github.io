# My Windows 8 Store App – Hitman Website Monitor

After creating my [Windows Phone application][LINK1] I wanted to test out how difficult or easy it would be to port that to a Windows 8 Store App.  After some effort, I was able to get the Windows 8 Store App developed, tested, published, downloaded and installed.  See the tile in Figure 1.

![Benjamin’s Hitman website monitor tile][FIGURE1]
###### Figure 1, Benjamin’s Hitman website monitor tile

If you want to give it a try, you can download it [here][LINK2].

I already mentioned on my [Windows Phone blog][LINK1], some of the changes I needed to make in order to get the code working on the Windows 8 Store App that worked fine on the Windows Phone app.  There are 3 additional points I wanted to make with this article.

+ Binding SQLite to the default Windows Store Split App template data source
+ Inclusion of SQLite into the project for deployment
+ Privacy Statement

##Binding SQLite to a Windows Store Split App template

I was not able to easily find a place or method to store my data on the Windows 8 App, unlike on the Windows Phone application.  I ultimately chose SQLite and followed these [instructions][LINK3] to get it installed and configured.

At the same time I wanted to use the default Windows Store Split App template DataModel as shown in Figure 2.  As much of the template uses this data model, I felt confident that this was the best approach.

![Windows Store Split App template DataModel][FIGURE2]
###### Figure 2, Windows Store Split App template DataModel

The way I did this was when the Windows Store App starts up, I selected the data I wanted to display from the SQLite database I created, then bind it to the AllGroups ObservableCollection<T>.  I did this in the constructor of the SampleDataSource class.  The source snippet below is not supported nor endorsed by Microsoft.  It's just something I thought of myself.  Please make a comment if you find a better method.

```
public HitmanDataSource()
{ 
  //Load the data from the SQLite database into the DataModel
   var dbPath = Path.Combine(Windows.Storage.ApplicationData.Current.LocalFolder.Path, "hitman.db");
   using (var db = new SQLite.SQLiteConnection(dbPath))
  {
     List<HitmanWebsiteGroup> HitmanGroupList = db.Table<HitmanWebsiteGroup>().ToList();
    if (HitmanGroupList.Count() > 0)
    {
       foreach (var group in HitmanGroupList)
       {
          var hitmanGroup = new HitmanDataGroup(group.UniqueId, group.Title, group.SubTitle, group.ImagePath, group.Description);
          List<HitmanWebsiteItem> HitmanItemList = db.Table<HitmanWebsiteItem>().Where(i=>i.GroupId == group.UniqueId).ToList();
          if (HitmanItemList.Count() > 0)
          {
            foreach (var item in HitmanItemList)
            {
              hitmanGroup.Items.Add(new HitmanDataItem(item.UniqueId, item.Title, item.SubTitle, item.ImagePath,…));
            }                           
          }
          this.AllGroups.Add(hitmanGroup);
       }   
     }
   }
}
```

After that, the data stored in my SQLite database is loaded into my classes and are fully integrated into the data model logic.  All of the ‘Page state management’ and ‘Logical page navigation’ methods, load and save the data as expected.

## Inclusion of the SQLite into the project

It is likely that I manually configured the SQLite package into my Solution, because my first attempt to get the application certified failed with a status of 3.2.  The description of all failures can be found [here][LINK4].

The application did hang when it was tested because I was not including SQLite with the Windows 8 App deployment package.  This was easy to fix, I just needed to include the ‘SQLite for Windows Runtime’ as a reference.  This is shown in Figure 3.

![Add ‘SQLite for Windows Runtime’ as a reference to your Windows Store App][FIGURE3]
###### Figure 3, Add ‘SQLite for Windows Runtime’ as a reference to your Windows Store App

That was pretty easy to solve, but not necessarily easy to find based on the 3.2 description:  Your app must not stop responding, end unexpectedly, or contain programming errors. 

The way I troubleshot it was to comment out the code I showed above, created the deployment package and deployed it directly to my Surface, via USB stick.  After some attempts I found the problem and it worked.  I narrowed things down a bit and was ultimately able to realize what I had done. 

## Privacy Statement

Another certification failure I received was 4.1: Your app must comply with the following privacy-related requirements.  I read through the description of that and made the modification to my app that added a link to my Privacy Statement when the Settings charm get activated, as shown in Figure 4.

![Windows 8 App - Privacy Statement][FIGURE4]
###### Figure 4, Windows 8 App - Privacy Statement

## Conclusion

The free version of my application, both the Windows Phone version and the Windows Store version are complete.  Since both of the platforms can be written in C#, there was relatively little work to do so that the functionality worked on both devices.  The navigation was a little different and that required some design changes, but I learned a lot and hope others find this SQLite to DataModel binding code snippet useful, plus my other comments.

[FIGURE1]: ../images/2013/msdn-0225.png "Figure 1, Benjamin’s Hitman website monitor tile"
[FIGURE2]: ../images/2013/msdn-0226.png "Figure 2, Windows Store Split App template DataModel"
[FIGURE3]: ../images/2013/msdn-0227.png "Figure 3, Add ‘SQLite for Windows Runtime’ as a reference to your Windows Store App"
[FIGURE4]: ../images/2013/msdn-0228.png "Figure 4, Windows 8 App - Privacy Statement"

[LINK1]: ../2012/2012-10-my-windows-phone-app-hitman-website-monitor.md
[LINK2]: http://apps.microsoft.com/windows/en-US/app/hitman-website-monitor/c5be98d8-9551-4164-a371-2f2ea74a45d6
[LINK3]: http://timheuer.com/blog/archive/2012/08/07/updated-how-to-using-sqlite-from-windows-store-apps.aspx
[LINK4]: http://msdn.microsoft.com/en-us/library/windows/apps/hh694083.aspx
