---
title: HW9
---

# Homework 9

Homework 9 takes homework 8 and deploys it into the cloud.

Step 1: Login to azure and create an sql server under the sql server menu. Fill out the information on the create page and add a database to store your tables in.

!["Create Server"](/img/dbDash.png)

Step 2: Connect to the remote server in SMSS and run the up.sql script from HW8. This will populate the tables in the database.

!["SMSS"](/img/uploaddb.png)

Step 3: Go back to Azure and create a new app under app services. Fill out the information. Initially I had made a mistake on this page. You will need to select web app + sql. I selected web app, and even though it has a section for connection strings it does not contain the authentication portion for the sql server. Also make sure the resource group is the same as the sql server.

I also made and erd. All I had to do was click create on the database.

!["ERD"](/img/erd.png)

Mistake:

!["Mistake"](/img/CreateApp.png)

The right way:

!["Web App"](/img/Create the Webapp sql.png)

!["App Options"](/img/AppOptions.png)

Step 5: Publish your app on Azure. Open your app and right click over the project in the solution explorer. Click publish, and enter the information for your account and resource group.

!["Publish"](/img/PublishOptions.png)

Step 6: Publish and run the app. (Happy dance optional)

!["App"](/img/Deployed.png)

Link to HW9 on Azure: [HW9](http://homework8.azurewebsites.net)

## Interesting Things and Helpful Websites

I was having a lot of trouble with the connection strings on my web app. For some reason a setting got changed in my publish settings when I went to upload it to Azure. I wasn't initially able to tell what was wrong. I eventually found a page that helped me turn on default error logging in MVC. It is by defualt set to off to prevent the world from seeing critical erors. I found out that it did not display any sensitive information but did display the critical error page.

The code (gets added in the web.config file):

```html
<system.web>
    <customErrors mode="Off"/>
</system.web>
```

Link to the website with the code: ["error"](https://blogs.msdn.microsoft.com/waws/2014/05/22/azure-web-sites-troubleshooting-server-error-in-application-errors/)

I also found a helpful link for deploying a web app on Azure with a database on MSDN. It was a little out dated but still had relevant information in it: ["Azure"](https://msdn.microsoft.com/en-us/library/dd483479(v=vs.100).aspx)

An interesting thing I ran into with the connection string that I ran into was my VS configuration was set to override the web connection string. It took me hours to find the problem and it eventually led to me playing with the settings menu in the publish menu. I found out that you could also set the connection string on the VS end without messing with the connection strings in the Azure menus. I didn't do it that way because Azure was easier but it was very easy to override your default context to accept the new connection string on load.

Image of menu: !["publish settings](/img/Publish Options.png)

