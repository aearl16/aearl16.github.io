---
title: HW5
---
# Homework 5

I found this homework particularly challenging. There were many peices to manage and a ton of new things to learn. I got the gist after fiddling with the code and checking the examples. You create a Model for your database. This is esentially an entity that will belong in the database. The database gets created with the fields that belong to the Model. The Context get created in the DAL. This is the setter and getter for each of the items in the database.

I started as usual but this time I decided to create the branches first based on what features I was going to start first.

```shell
git branch HW5_Form
git branch HW5_SQL
git branch HW5_Conetext
git branch HW5_Views
git branch HW5_Model
git checkout HW5_Model
```

I put the .gitignore into the directory and then pushed to the remote.

```shell
git status
git add .
git commit - m "added gitignore"
git push --set-upstream origin HW5_Model
```

I created the Model and then pushed to the branch again.

```csharp
namespace Homework5.Models
{
    /// <summary>
    /// DMV Customer Model
    /// </summary>
    public class Customer
    {
        /// <summary>
        /// Customer ID, used as PK
        /// </summary>
        public int CustomerID { get; set; }

        /// <summary>
        /// Date of Birth
        /// </summary>
        [Required]
        public DateTime DOB { get; set; }

        /// <summary>
        /// First Name
        /// </summary>
        [Required, StringLength(64)]
        [Display(Name = "First Name")]
        public string FirstName { get; set; }

        /// <summary>
        /// Last name
        /// </summary>
        [Required, StringLength(64)]
        [Display(Name = "Last Name")]
        public string LastName { get; set; }

        /// <summary>
        /// Middle Name
        /// </summary>
        [Required, StringLength(64)]
        [Display(Name = "Middle Name")]
        public string MiddleName { get; set; }
```

    Note: To long to add the rest of the class.

The next piece was created the Database with SQL. I did exactly the same as Example 2 but made sure all of my fields were present and inserted with the information for my table. I also created the DOWN.sql to drop the table.

```sql
-- Insert Statement
INSERT INTO dbo.Customers (LastName, FirstName, MiddleName, DOB, CAddress, City, CState, Zipcode, MailingAddress, MailingCity, MailingState, MailingZip) VALUES 
    ('Smith', 'John', 'Jacob', '1940-4-21', '1642 Columbus Way', 'Columbus', 'Ohio', 97442, '', '', '', 0),
    ('Bot', 'Hax', 'The','2942-12-12', '2001 Binary Rd.', 'Colony X', 'Mars', 101010, 'PO Box 16', 'Mars City', 'Mars', 101010);

GO
```

I then made the context. I couldn't get this to work at first but then someone reminded me on slack that I had to use the NuGet pkg manager to get the .NET MVC SQL context package. I had totally forgot about that piece. One frustrating piece about this is if you create a DAL file with nothing in it, Visual Studio default hides the empty and invisible files. There is a tiny folder button at the top of the Solution Explorer that says show all files. Once I found that I was able to add my context file to it.

```csharp
namespace Homework5.DAL
{
    /// <summary>
    /// Customer Context for DMV
    /// </summary>
    public class CustomerContext : DbContext
    {
        /// <summary>
        /// Superclass Constructor
        /// </summary>
        public CustomerContext() : base("name=OurDBContext")
        { }

        /// <summary>
        /// This is the getter and setter method for each Customer in Customers
        /// </summary>
        public virtual DbSet<Customer> Customers { get; set; }
    }
}
```

Next was the forms and the HTML pages. They are too long and extensive to put in here so please see my files in the repo. The interesting thing I found by createing the views using the model and context is that if you rename the CRUD cshtml files with a different name, the links created in the details page will not link to the correct page. The auto-generated code didn't account for this leading to my CRUD pages to return a 404. Once I figured out that the URL in the bar wasn't the actual link to my page I fixed the nameing to match the links it wanted. I cold've fixed the links in the CustomerDetails cshtml page but I didn't want other pieces of my code to malfuntion, so I just renamed the pages.

The final piece was fixing the Controller and adding comments. Most of the CRUD methods required special set and get methods related to the Primary Key. I at first just generated the views to see what the MVC controller would return. Unfortunately they all returned a 400, since the pages weren't actually linked to the DB Context. I then went to work making sure that all of my pieces were exactly the same as Example 2 (With my naming and classes) so that I could make sure it all worked.

Excerpt from the Controller:

```csharp
    /// <summary>
    /// Main controller for DMV forms
    /// </summary>
    public class HomeController : Controller
    {
        /// <summary>
        /// The context for my Customer Model
        /// </summary>
        private CustomerContext cc = new CustomerContext();

        public ActionResult Index()
        {
            return View();
        }

        // GET: Home/CustomerDetails/2
        /// <summary>
        /// Details for a specific customer
        /// </summary>
        /// <param name="id"></param>
        /// <returns></returns>
        public ActionResult Details(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Customer cust = cc.Customers.Find(id);
            if (cust == null)
            {
                return HttpNotFound();
            }
            return View(cust);
        }
```

## Screenshots of working program:

!["Main App Page"](img/HW5 App.png)

!["SQL DB View Page"](img/HW App2.png)