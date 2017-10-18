---
title: HW2
---

## Homework 2

Homework two was an introduction to javascript. I decided to make a page that had draggable elements. There were some examples online. On of the most helpful was [https://www.w3schools.com/html/html5_draganddrop.asp](https://www.w3schools.com/html/html5_draganddrop.asp). It was a lot of fun to figure out, but the alignments and containers were a little frustrating. I used my default page setup from my main site and simplified the javascript app page. I used the DOM to locate the IDs of the elements and set the draggable property.

I created my local repo with the usual:

```shell
git init
git add .
```
- I added my empty files here and mad a couple of setup commits
```shell
git push remote origin
```
Then I made my new branch
```shell
git branch HW2_feature
```

I copied my general page setup from HW1 so most of the main page code is the same. I plan to fix some of the styling on my page when I get a chance.

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		 <title>Aaron Earl</title>
		 <meta charset="utf-8">
		 <meta name="viewport" content="width=device-width, initial-scale=1">
		 <link rel="stylesheet" href="css/bootstrap.css">
		 <link href="https://fonts.googleapis.com/css?family=Ubuntu" rel="stylesheet">
		 <script src="jquery/jquery-3.2.1.js"></script>
		 <script src=https://unpkg.com/popper.js></script>
		 <script src="js/bootstrap.js"></script>
		 <nav class="navbar navbar-expand-sm navbar-dark" style="background: #D83F07;">
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#nav-content" aria-controls="nav-content" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
			
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
					<a class="nav-link" href="page1.html"> Javascript Demo </a>
				</li>
			</ul>
		
		</nav>
	</head>
	<br></br>
	<body>

		<br></br>
		<div class="container">
				
			<div class="jumbotron">
			<div align="center">
				<h1> Welcome! </h1>
			</div>
			<br></br>
			<p>
				Welcome to my page. I created this for my CS460 class. My hope is to learn
				html, css, bootstrap and other parts of web development to further my photography work
				as well as my projects at Intel. I am looking forward to the school year and 
				looking forward to learning as much as I can.
				<br></br>
				Thanks for visiting!
				<br></br>
				Aaron Earl
			</p>
			</div>
				
		</div>

	</body>
</html>
```

The next piece was creating the new page and adding the draggable elements. Images are draggable by defualt for some reason. I had to change the default draggable value for my image to false. That way the functionality could stay the same.

- This is my header with my defined scripts, css, and other elements

```html
 <title>Aaron Earl</title>
		 <meta charset="utf-8">
		 <meta name="viewport" content="width=device-width, initial-scale=1">
		 <link rel="stylesheet" href="css/bootstrap.css">
		 <link href="https://fonts.googleapis.com/css?family=Ubuntu" rel="stylesheet">
		 <script src="jquery/jquery-3.2.1.js"></script>
		 <link rel="stylesheet" href="css/mystyle.css">
         <script src=https://unpkg.com/popper.js></script>
         <script src=js/myscript.js></script>
		 <script src="js/bootstrap.js"></script>
		 <nav class="navbar navbar-expand-sm navbar-dark" style="background: #D83F07;">
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#nav-content" aria-controls="nav-content" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
```

```html
    <div class="container">
            
        <div class="jumbotron">
        <div align="center">
            <h1> Welcome! </h1>
        </div>
        <br></br>
        <p>
            This page will demo javascript drag and drop items. It uses the function from 
            jquery to get the docuement items by ID. To use this page, click which items you 
            would like to make draggable and drag them into the boxes.Reload to reset.
        </p>
        <ul>
            <li><input id="pDragger" onclick=setAttrP() type="checkbox">  Set Paragraphs Draggable</li>
            <li><input id="tDragger" onclick=setAttrT() type="checkbox"> Set Text Draggable</li>
            <li><input id ="imgDragger" onclick=setAttrImg() type="checkbox"> Set Image Draggable</li>
        </ul>
        </div>
    <br><br> 

	<body>
    <div align="center">
        </div>
            <div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
            <br></br>
            <div id="div2" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
            <br></br>
            <div id="div3" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
            <br></br>
            <p id="pg1" ondragstart="drag(event)" width="336" height="69">
                This paragraph is draggable.
            <p>
            <br><br>
            <img id="drag1" draggable="false" src="img/woulogo.png" ondragstart="drag(event)" width="336" height="69">
            <br></br>
            <div id="drag2" ondragstart="drag(event)">
                This is plain text.
            </div>
            <br></br>
    </div>
```
I made the defualt page elements but then I needed to make the javascript methods to move the elements around. Most of the javascript just calls properties already available in the obejct tags. I apologize in advance. I meant to comment the js code but I forgot. The main gist is that a drag and a drop event occurs. As the element is dragged accross the page, nothing happens. The second event occurs when the element is dropped. It is then appended to the object that it is dropped in. By defualt I have set it to the custom divs; they listen for the drop event and run drop() after they have detected it. The last three methods set the draggable attribute on the user selected elements.

```javascript
function allowDrop(ev) {
    ev.preventDefault();
}

function drag(ev) {
    ev.dataTransfer.setData("text", ev.target.id);
}

function drop(ev) {
    ev.preventDefault();
    var data = ev.dataTransfer.getData("text");
    ev.target.appendChild(document.getElementById(data));
}

function setAttrP()
{
    var check1 = document.getElementById("pDragger");
    var drag1 = document.getElementById("pg1");
    if(check1.checked){
        drag1.setAttribute('draggable', true);
    }
    else{
        drag1.removeAttribute('draggable');
    }
}

function setAttrT()
{
    var check2 = document.getElementById("tDragger");
    var drag2 = document.getElementById("drag2");
    if(check2.checked){
        drag2.setAttribute('draggable', true);
    }
    else{
        drag2.removeAttribute('draggable');
    }
}

function setAttrImg()
{
    var check3 = document.getElementById("imgDragger");
    var drag3 = document.getElementById("drag1");
    if(check3.checked){
        drag3.setAttribute('draggable', true);
    }
    else{
        drag3.removeAttribute('draggable');
    }
}
```
The last peice was adding some styling to the blank divs I created for dragging and appending the elements to.

```css
#div1, #div2, #div3 
{
    width: 350px;
    height: 70px;
    padding: 10px;
    border: 1px solid #000000;
}
```
The final piece of this assignment was merging the feature branch into master.

```shell
git checkout master
git merge HW2_feature
git push origin master
git stash
git pull
```

I stashed the master branch so I could pull soen the merged code and prevent untracked files from causing conflicts.