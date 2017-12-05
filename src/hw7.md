---
title: HW7
---

# Homework 7

Homework 7 introduced a web application that used an api key to query data from Giphy. The query would contain json that needed to be parsed and returned to the user of the app with the information that was desired. A logging database would also be use to record the queries that were requested along with some data from the uers.

Setting up I created the MVC application and got my main controller set up.

```csharp
using Homework7.DAL;
using Homework7.Models;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;

namespace Homework7.Controllers
{
    public class GiphyController : Controller
    {
        //Database Context
        SearchDataContext db = new SearchDataContext();

        // GET: Giphy
        /// <summary>
        /// Main page
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            return View();
        }

        // GET: Search
        /// <summary>
        /// Giphy Search
        /// </summary>
        /// <returns></returns>
        [HttpGet]
        public JsonResult Search()
        {
            List<GiphyResult> list = new List<GiphyResult>();
            //URI to be built base on input
            string key = System.Web.Configuration.WebConfigurationManager.AppSettings["GiphyAPIKey"]; //get the key
            string httpQuery = "http://api.giphy.com/v1/gifs/search?api_key="
                             + key
                             + "&q=" 
                             + Request.QueryString["find"]
                             + "&rating="
                             + Request.QueryString["rating"];

            //New web request with the URI above
            WebRequest dataRequest = WebRequest.Create(httpQuery);
            //Get the response stream from Giphy
            Stream dataStream = dataRequest.GetResponse().GetResponseStream();

            var returnedData = new System.Web.Script.Serialization.JavaScriptSerializer()
                                   .Deserialize<RootObject>(new StreamReader(dataStream)
                                   .ReadToEnd());
            dataStream.Close();

            for (int i = 0; i < 25; i++)
            {
                GiphyResult result = new GiphyResult();
                //Default Image size will be medium
                result.image = returnedData.data[i].images.downsized_medium.url;
                list.Add(result);
            }

            //Get the querying user's information
            string ipAddress = Request.UserHostAddress;
            string userAgent = Request.UserAgent;
            string search = Request.QueryString["find"];

            //New DataLog object for storing the user's information
            DataLog dl = new DataLog();

            //Store the querying user's information
            dl.IPAddress = ipAddress;
            dl.Agent = userAgent;
            dl.SearchDate = DateTime.Now;
            dl.SearchRequest = search;

            //Add the object to the database
            db.datalogs.Add(dl);
            db.SaveChanges();

            //Return the results to the calling method
            return Json(list, JsonRequestBehavior.AllowGet);
        }
    }
}
```

I created models and context for the databse and created a custom model for the Giphy json called giphy result. I used an open source web tool to generate the class from the json. Link to the tool: ["Json2CSharp"](http://json2csharp.com/).

My Up Script for the logging database:

```sql
CREATE TABLE datalog
(
	ID INT IDENTITY (1,1) NOT NULL,
	SearchRequest VARCHAR(MAX) NOT NULL,
	Agent VARCHAR(MAX) NOT NULL,
	IPAddress VARCHAR(128),
	SearchDate DateTime NOT NULL,
	CONSTRAINT [PK_datalog] PRIMARY KEY (ID)
);
```

Next I created a custom route for my get method.

```csharp
routes.MapRoute(
                name: "MyRoute",
                url: "{Giphy}/{action}/{find}",
                defaults: new { Giphy = "GiphyController", action = "Search", find = UrlParameter.Optional}
            );
```

One thing to note that was interesting: I made this change after I had created my Javascript files and Visual Studio was smart enough to make the URL change for me.

I created my javascript files to handle the Ajax request as well as the jQuery for changing the size selector for the images.

```javascript
/***************************************************************************
* This method will call the C# controller method based on the search click.*
* It will add the user's input from the text box and put it into the query *
* string to pass to the controller. It will get the returned data and      *
* call the displayStuff method to append images to the display area        *
***************************************************************************/
$("#searchButton").click(function () {
    $.ajax({
        type: "GET",
        dataType: "json",
        url: "/Giphy/Search?find=" + $("#searchBox").val() +"&rating=" + $("#rating").val(),
        success: function (data) {
            displayStuff(data);
        },
        error: function (data) {
            alert("There was an error. Try again please!");
        }
    });

});

/*//////////////////////////////////////////////////////////////////////////
* Empties the display area and appends the giphy return data to the        *
* display area. Then calls the set size method to fix the size based on    *
* ths user's input into the drop down menus                                *
* input: data | the data from the Giphy Json                               *
*///////////////////////////////////////////////////////////////////////////
function displayStuff(data) {
    $("#displayArea").empty();
    $.each(data, function (i, item) {
        $("#displayArea").append("<div class='giphy-container'><img class='img-responsive' src='" + item["image"] + "' /></div>")
    });
    setSize();
}

//If size changes change the set size of the images
$("#size").change(setSize); 

/*
* Sets image size actively using JQuery
*/
function setSize() {
    $(".giphy-container").removeClass("container-sm");
    $(".giphy-container").removeClass("container-md");
    $(".giphy-container").removeClass("container-lg");
    /***********************************************************************
     * add a class to the container based on the #size select element      *
     *      #size value |   string appended                                *
     *      "sm"        |   container-sm (default)                         *
     *      "md"        |   container-md                                   *
     *      "lg"        |   container-lg (final)                           *
     ***********************************************************************/
    $(".giphy-container").addClass(($("#size").val() == "sm")
                    ? "container-sm"
                    : (($("#size").val() == "md")
                    ? "container-md" : "container-lg"));
}
```

The last part was adding the finishing touches like the css and hiding the API key.

My CSS:

```css
/* Set padding to keep content from hitting the edges */
.body-content {
    padding-left: 15px;
    padding-right: 15px;
}

/* Set width on the form input elements since they're 100% wide by default */
input,
select,
textarea {
    max-width: 280px;
}

/*Format containers*/
.giphy-container {
    float: left;
    margin: 10px;
}

.container-sm {
    width: 200px;
}
.container-md {
    width: 300px;
}
.container-lg {
    width: 400px;
}
```

## Hiding the API Key

Hiding the API key was easy. I found this tutorial on MSDN very helpful: ["MSDN Hiding Resources Tutorial"](https://docs.microsoft.com/en-us/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure) as well as this page on appSettings tags in MVC: ["appSettings"](https://msdn.microsoft.com/en-us/library/aa903313(v=vs.71).aspx).

Getting the key from AppSettings:

```csharp
string key = System.Web.Configuration.WebConfigurationManager.AppSettings["GiphyAPIKey"]; //get the key
```

The config file:

```xml
<appSettings file="C:\Users\Aaron Earl\Documents\API Key\appAPI.config">
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
  </appSettings>
```

What is in the appAPI.config file:

```xml
<appSettings>
	<add key="GiphyAPIKey" value="Ah Ah Ah Didn't Say the Magic Word!"/>
</appSettings>
```

## Program in operation:

!["HW7Run"](/img/HW7SearchSM.png)

!["HW7Run2"](/img/HW7SearchMD.png)

!["HW7Run3"](/img/HW7SearchLG.png)

## The logfile:

!["HW7Log"](/img/HW7Log.png)