---
title: HW4
---
# Homework 4

Again this homework started the same as usual but with the exception of creating all of the branches for each piece.

```shell
git branch HW4_feature
git branch HW4_Calculator
git branch HW4_Page2
git branch HW4_Links
```

I created the branches first and then checked them out as needed. I made a mistake with HW4_feature. I at first called my first part of the app Page1 and then changed it. Later as I went to merge I forgot it and mislabeled the merge. My merge for Page1 was actually my merge for Page2. This is reflected in the log as one merge is missing. With this many branhes it was getting easier to lost track of them.

The MVC portion was kindo of difficult to understand even with the example. There are many peices to it and I had some trouble figuring out how they meshed together. I set up the initial empty project and made the pages with links in the default _Layout page.

```html
<li>@Html.ActionLink("Home", "Index", "Home")</li>
<li>@Html.ActionLink("App", "Page2", "Home")</li>
<li>@Html.ActionLink("Loan Calculator", "LoanCalculator", "Home")</li>
<li>@Html.ActionLink("About", "About", "Home")</li>
<li>@Html.ActionLink("Contact", "Contact", "Home")</li>
```

After I created that piece I merged it with master and proceeded to the next piece. I finally figured out how the query string interacted with the controller page and was able to get the simple query working for page one. As I stated above I accidentally messed up the merge for page one due to both my naming of the branch and changing the the page name, so I missed the merge and commit for page one. I called Page1 the SimpleQuery since it used a simple url query.

```html
@{
    ViewBag.Title = "SimpleQueryView";
}

<h2> Hi @Model.FirstName @Model.LastName !</h2>
```

```csharp

/// <summary>
/// Get page for the simple query
/// </summary>
/// <returns></returns>
[HttpGet]
public ViewResult SimpleQueryView()
{
    return View();
}
```

The next page had a similar setup but used a simple form in html. I used the FormCollection array to assign values to the ViewBag. There is more Razor code in thie View than some of the others. It Checks to see if the user has entered their name properly and displays a different result depending on what was entered.

```html
@{
    ViewBag.Title = "Test App";
}

<h2>Test Web Form</h2>

<form action="~/Home/Page2" method="post">
    First Name:<br />
    <input type="text" name="FirstName" value=""/>
    <br />
    Last Name:<br />
    <input type="text" name="LastName" value="" />
    <br />
    <br />
    <input type="submit">
</form>

@if(ViewBag.FirstName == null & ViewBag.LastName == null)
{
    @: You forgot to enter your name!
}
else if(ViewBag.FirstName == null)
{
    @: You forgot your first name!
}
else if(ViewBag.LastName == null)
{
    @: You forgot your last name!
}
else
{
    <p> Hi @ViewBag.FirstName @ViewBag.LastName! </p>
}
```

```csharp
        /// <summary>
        /// Get page for the simple query
        /// </summary>
        /// <returns>SimpleQueryView</returns>
        [HttpGet]
        public ViewResult SimpleQueryView()
        {
            return View();
        }

        /// <summary>
        /// This method is the post for Page2
        /// </summary>
        /// <param name="fc"></param>
        /// <returns>Page2 Post</returns>
        [HttpPost]
        public ViewResult Page2(FormCollection fc)
        {
            ViewBag.FirstName = fc["FirstName"];
            ViewBag.LastName = fc["LastName"];
            return View();
        }
```

The last piece was the Loan Calculator with strong typing to the input objects. I created a simple calculator in C# and then had it post and display its calculations back to the same page for the user to see. At the time of this writing I am still trying to figure out a Regex to use and ensure that a proper input is accepted. I plan to add additional Razor code for this depending on how it should be handled. For now I wanted to show the code for the calculator and how I got it to display. I did it slightly similar to Example 2 from the class repository. I was suprised by how short the code could be for showing some input information.

```html
@{
    ViewBag.Title = "LoanCalculator";
}

<h2>Loan Calculator</h2>

<form action="~/Home/LoanCalculator" method="post">
    Down Payment:<br />
    <input type="text" name="downPayment" value="" />
    <br />
    Purchase Price:<br />
    <input type="text" name="purchasePrice" value="" />
    <br />
    Interest Rate:<br />
    <input type="text" name="iRate" value="" />
    <br />
    Loan Term(In Months):<br />
    <input type="text" name="term" value="" />
    <br />
    <br />
    <input type="submit">
</form>

@if(ViewBag.MonthlyPayment != null)
{
    <p>Your monthly payment is: @ViewBag.MonthlyPayment</p>
}
```

This is the actual calculator in C#. It allows 0 to be input for the interest rate (assuming its a loan without interest) and divides it by 12 (number of months in the year). If 0 is not entered it does a typical calculation.

```csharp
        /// <summary>
        /// Post method for the Loan Calculator
        /// </summary>
        /// <param name="iRate"></param>
        /// <param name="purchasePrice"></param>
        /// <param name="downPayment"></param>
        /// <param name="term"></param>
        /// <returns>Loan Calculator</returns>
        [HttpPost]
        public ViewResult LoanCalculator(decimal iRate, decimal purchasePrice, decimal downPayment, int term)
        {
            const int MonthsInYear = 12;
            decimal payment = 0;
            decimal loanAmt = purchasePrice - downPayment;
            int numYrs = term / MonthsInYear;

            if(numYrs > 0)
            {
                if(iRate != 0)
                {
                    decimal rate = (iRate / MonthsInYear) / 100;
                    decimal factor = (rate + (rate / (decimal)(Math.Pow((double)rate + 1, term) - 1)));
                    payment = (loanAmt * factor);
                }
                else
                {
                    payment = loanAmt / term;
                }
            }
            payment = Math.Round(payment, 2);
            ViewBag.MonthlyPayment = payment;

            return View();
        }
```

## Pictures of the main pages in operation

![Simple URL Query](/img/MVCSimple.png)

![Form Page](/img/MVCPage2.png)

![Loan Calculator](/img/MVCLoanCalc.png)