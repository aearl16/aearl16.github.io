---
title: HW9
---
#Homework 9

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