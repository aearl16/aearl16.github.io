---
title: HW6
---

This homework demonstrated the use of a complex database built into a web app. The web app behaved like a store website where a user would select the items on a page and view the products. The database would be queried using Linq queries to return and display the results. The app also allowed users to create product reivews for a given product.

The first part of this assignment was the real challenge. Getting the database restore from the backup .bak file. I used SMSS to restore the database but accidentally selected the whole database and not just the Product portion I needed. I deleted that project and started again. After I was able to connect to the internal database I connected to it in LinqPad to test queries and verify the results I expected.

After that I began generating the models and context. I made my own context but for some reason had trouble accessing the model keys. Visual Studio gave me a large error with an ok message when I tried to build. I evnetually gave up on that and had Visual studio generate the context.

My home controller managed all of the model binding for create and display. I accidentally forgot my get method early on and ran into a 404 when I tried to display my create page for a reivew. I passed around the IDs for each of the queries that way the end user didn't have to worry about looking it up or not knowing it.

 Home controller:

 ```csharp
 using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;
using Homework6.Models;
using System.Net;

namespace Homework6.Controllers
{
    public class HomeController : Controller
    {
        /// <summary>
        /// Global variables
        /// </summary>
        private AdventureWorks2014Entities db = new AdventureWorks2014Entities();
        private int PageSize = 4;

        /// <summary>
        /// Homepage
        /// </summary>
        /// <returns> Returns Index.cshtml view</returns>
        public ActionResult Index()
        {
            return View();
        }

        /// <summary>
        /// Queries Categories and displays subcategories based on query
        /// Defaulst to Bikes and Page 1
        /// </summary>
        /// <param name="category"></param>
        /// <param name="page"></param>
        /// <returns> View of subcategories </returns>
        [HttpGet]
        public ActionResult Categories(string category = "Bikes", int page = 1)
        {
            return View(db.ProductSubcategories
                        .Where(v => v.ProductCategory.Name.Contains(category))
                        .OrderBy(p => p.ProductCategoryID)
                        .Skip((page - 1) * PageSize)
                        .Take(PageSize));

        }

        /// <summary>
        /// Dispalys products based on Subcategory Queries
        /// </summary>
        /// <param name="ID"></param>
        /// <param name="page"></param>
        /// <returns> View of Products based on ProductSubcategory ID</returns>
        [HttpGet]
        public ActionResult Products(int ID = 1, int page = 1)
        {
            return View(db.Products
                        .Where(catID => catID.ProductSubcategoryID == ID).ToList());

            //Tried Pagination
            /**
            return View(db.Products
                        .Where(catID => catID.ProductSubcategoryID == ID)
                        .OrderBy(pID => pID.ProductID)
                        .Skip((PageSize - 1) * PageSize)
                        .Take(PageSize));
            */
        }

        [HttpGet]
        public ActionResult ProductDetails(int? ID, int page = 1)
        {
            if(ID == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            else
            {
                return View(db.Products
                            .Where(p => p.ProductID == ID));
            }
        }

        /// <summary>
        /// Retrieves and gets the reivews based on ProductID
        /// </summary>
        /// <param name="ID"></param>
        /// <returns> View of ProductReivews </returns>
        [HttpGet]
        public ActionResult Reviews(int? ID)
        {
            if (ID == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            else
            {
                return View(db.ProductReviews
                            .Where(p => p.ProductID == ID)
                            .OrderByDescending(d => d.ReviewDate).ToList());
            }
        }

        /// <summary>
        /// Get method for Create Reivew
        /// </summary>
        [HttpGet]
        public ActionResult Create()
        {
            return View();
        }

        /// <summary>
        /// Creates and Posts a review
        /// </summary>
        /// <param name="review"></param>
        /// <returns> Redirect to the Index page whether or not a reviewis posted</returns>
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ProductID, ReviewerName, ReviewDate, EmailAddress, Rating, Comments, ModifiedDate")] ProductReview review, int ID)
        {
            review.ProductID = ID;
            review.ReviewDate = DateTime.Now;
            review.ModifiedDate = DateTime.Now;

            if (ModelState.IsValid)
            {
                db.ProductReviews.Add(review);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View("Index");
        }
    }
}
```

The passing of parameters through each of the pages:

```html
<td>
    @Html.ActionLink("Details", "ProductDetails", new { ID = item.ProductID })
</td>
```

One of the interesting things I was able to figure out when creating a review was that if the model was bound it could also have values passed to it. To prevent the user from having to deal with time, date, and ID for each of the products reviewed I passed ID from the model, then used it to upload to the database by that particular ID.

```html
<td>
    @Html.ActionLink("Reviews", "Reviews", new { ID = item.ProductID}, "Home")
    @Html.ActionLink("Add Review", "Create", new { ID = item.ProductID}, "Home")
</td>
```
This allowed both viewing and creation of a review by ProductID.

The program in operation:

!["Main App Page"](/img/HW6_App.png)

!["Product Category Page"](/img/HW6_App2.png)

!["Product Subcategory Page"](/img/HW6_App3.png)

!["Product Details Page"](/img/HW6_App4.png)

!["List of Product Reivews for a Product"](/img/HW6_App5.png)

!["LinqPad query to show review in DB"](/img/HW6_App6.png)