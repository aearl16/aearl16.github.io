---
title: HW1
---

## Homework 1

Homework one was an introduction to HTML5, CSS3, Bootstrap, and general web design. I am not very proficient at web related things so I had to do a bit of research.

## Steps:

### Git

I already had git installed on my machines so I just upated it to the latest version. I am already fairly used to using it so I just got set up and got my repo ready to go. I hadn't done a git init with a git add branch before and I may try that with my dev branch when I get to it. For this initial repo I just made one on Github and cloned it on my machine. I also created a directory to store my stuff on my local machine and my laptop.

**Did this on both Machines**
```shell
cd Documents
mkdir CS460_Web
git clone https://github.com/aearl16/CS460_Web.git
git status
```
I started some files then
```shell
git add src
git commit -a -m "added src and initial html files"
git push
```
Second step was to begin writing some html files. I started with the navbar because it was the easiest. I later realized it didn't work because bootstrap has changed to 4.0. Bootstrap 4.0 is different and classnames are different so I had to do some research. I eventially did this:

```html
<title>Aaron Earl</title>
		 <meta charset="utf-8">
		 <meta name="viewport" content="width=device-width, initial-scale=1">
		 <link rel="stylesheet" href="css/bootstrap.css">
		 <link href="https://fonts.googleapis.com/css?family=Ubuntu" rel="stylesheet">
		 <script src="jquery/jquery-3.2.1.js"></script>
		 <link rel="stylesheet" href="css/mycss.css">
		 <script src=https://unpkg.com/popper.js></script>
		 <script src="js/bootstrap.js"></script>
		 <nav class="navbar navbar-expand-sm navbar-dark" style="background: #D83F07;">
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#nav-content" aria-controls="nav-content" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
		
			<!-- Brand -->
			<a class="navbar-brand" href="index.html">
				<img class="rounded" src="img/woulogo.png" height="45px">
			</a>
			
			<!-- Links -->
			<div class="collapse navbar-collapse" id="nav-content">   
			<ul class="navbar-nav">
				<li class="nav-item">
					<div align="center">
						<a href="index.html">
							<svg width="35px" height="35px" viewBox="0 0 16 16" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
								<!-- Generator: Sketch 40.3 (33839) - http://www.bohemiancoding.com/sketch -->
								<title>home</title>
								<desc>Created with Sketch.</desc>
								<defs></defs>
								<g id="Octicons" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
									<g id="home" fill="#000000">
										<path d="M16,9 L13,6 L13,2 L11,2 L11,4 L8,1 L0,9 L2,9 L3,14 C3,14.55 3.45,15 4,15 L12,15 C12.55,15 13,14.55 13,14 L14,9 L16,9 L16,9 Z M12,14 L9,14 L9,10 L7,10 L7,14 L4,14 L2.81,7.69 L8,2.5 L13.19,7.69 L12,14 L12,14 Z" id="Shape"></path>
									</g>
								</g>
							</svg>
						</a>
					</div>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="page1.html"> My Schedule </a>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="page2.html"> Useful Links </a>
				</li>
				<li class="nav-item">
					<a class="nav-link" href="page3.html"> About Me </a>
				</li>
			</ul>
		
		</nav>
	</head>
```
**In this I also added the necessary files for bootstrap.**
  
After I got the initial menues working I added serveral pages for the pagination portion. Then I began working on the various list items and links. Several uls were used in menu items as show above:

```html
<div align="left">
			<h1> 
				<u> Useful Links: </u>
			</h1>
		</div>
		<br></br>
		<dl>
			<dt> Bootstrap: </dt>
			<ol>
				
				<li>
					Bootstrap 3: 
					<a href=https://www.w3schools.com/bootstrap/default.asp> 
						https://www.w3schools.com/bootstrap/default.asp
					</a>
				</li>
				<li>
					Bootstrap 4: 
					<a href=https://www.quackit.com/bootstrap/bootstrap_4/tutorial/> 
						https://www.quackit.com/bootstrap/bootstrap_4/tutorial/
					</a>
				</li>

			</ol>
			<dt> HTML5/CSS3: </dt>
				<ol>
					
					<li>
						HTML: 
						<a href=https://www.w3schools.com/html/default.asp> 
							https://www.w3schools.com/html/default.asp
						</a>
					</li>
					<li>
						CSS: 
						<a href=https://www.w3schools.com/css/default.asp> 
							https://www.w3schools.com/css/default.asp
						</a>
					</li>

				</ol>
				<dt> Fonts: </dt>
				<ol>
					
					<li>
						Ubuntu Web Font Tutorial: 
						<a href=http://font.ubuntu.com/web/> 
							http://font.ubuntu.com/web/
						</a>
					</li>
					<li>
						Google Font Reference: 
						<a href=https://fonts.google.com/> 
							https://fonts.google.com/
						</a>
					</li>

				</ol>
				<dt> Web Templates and Ideas: </dt>
				<ol>
					
					<li>
						HTML5 Up Templates: 
						<a href=https://html5up.net/> 
							https://html5up.net/
						</a>
					</li>
					<li>
						Picture in Paragraph: 
						<a href=http://www.createafreewebsite.net/paragraph_images.html> 
							http://www.createafreewebsite.net/paragraph_images.html
						</a>
					</li>
					<li>
						HTML5 VS 4: 
						<a href=https://stackoverflow.com/questions/134727/whats-the-key-difference-between-html-4-and-html-5> 
							https://stackoverflow.com/questions/134727/whats-the-key-difference-between-html-4-and-html-5
						</a>
					</li>
					
				</ol>
				<dt> C# (Coming Soon): </dt>
				<dt> MVC5 (Coming Soon): </dt>
		</dl>
```
I made a page of useful links to help me or anyone looking at the site. I will eventually add it to my public_html folder at school.

The last thing I did was pretty it up with my own css, add a table with my schedule, and add a custom font for fun:

```html
<table id="mytable">
			<tr>
				<th> Monday </th>
				<th> Tuesday </th> 
				<th> Wednesday </th>
				<th> Thursday </th>
				<th> Friday </th>
			</tr>
			<tr>
				<td> CS460: 8-8:50am </td>
				<td> CS460: 8-8:50am </td>
				<td> CS460: 8-8:50am </td>
				<td> CS460: 8-8:50am </td>
				<td> No Class </td>
			</tr>
			<tr>
				<td> CS363: 10-115:50am </td>
				<td></td>
				<td></td>
				<td> CS363: 10-115:50am </td>
				<td>No Class</td>
			</tr>
			<tr>
				<td></td>
				<td> BA211: 4-5:50pm </td>
				<td></td>
				<td> BA211: 4-5:50pm </td>
				<td> No Class </td>
			</tr>
		</table>
```

```html
<link href="https://fonts.googleapis.com/css?family=Ubuntu" rel="stylesheet">
```

**Custom CSS**
- The custom css performed a variety of functions. It formatted my table with my schedule, and defined my custom font for various elements withing the webpage.

```css
table {
    width:100%;
}
table, th, td {
    border: 1px solid black;
    border-collapse: collapse;
}
th, td {
    padding: 5px;
    text-align: left;
}
table#mytable tr:nth-child(even) {
    background-color: #eee;
}
table#mytable tr:nth-child(odd) {
   background-color:#fff;
}
table#mytable th {
    background-color: black;
    color: white;
}

p, h1, tr, th, a
{
    text-indent: 2.0em;
    font-family: 'Ubuntu', sans-serif;
}

body{
    background-color: #FFF8E7;
}
```

For the final touch I added an about page with some of my info on it. I am maybe going to add a contact page too. I made a jumbotron with an image embedded and floating to the left of the text:

```html
<div class="container">
				
			<div class="jumbotron">
			<div align="center">
				<h1> About Me </h1>
			</div>
			<br></br>
			<p>
				<img class="rounded" src="img/me.jpg" alt="alt text" style="float: left" height="200px">
				Hi my name is Aaron Earl and I'm a senior CS student at Western Oregon University. I am an intern
				in the Engineering Computing department at Intel. While I do a little bit of everything I have primarily
				been working with the Security Engineering team. I was always very fond of computers, even when I was young.
				I started school at Pacific Lutheran University as an EE major. When funding didn't work out I started over 
				and became a CS major and I am looking forward to graduating!
				<br></br>
				Some of my hobbies inclue Rasberry Pi, Aruduino, Robotics, Machine Learning, Circuits, drones, 
				photography, and fishing. I always have computers ripped apart in my house.
				I recently got into drones because of the book Make: Drones. And fishing because I like to eat fish of course!
				<br></br>
				Thanks for visiting!
				<br></br>
				Aaron Earl
			</p>
			</div>
				
		</div>
```

And that is my website! I still have a lot of things to figure out. I still can't seem to figure out the spatial padding for my picture and about page I added.
