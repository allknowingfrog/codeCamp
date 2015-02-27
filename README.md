## Pew-Pew - An HTML5, Vertical Scrolling Shooter

This tutorial series was originally presented at Sycamore Code Camp 2014. It is inspired by Mike Thomas's excellent tutorial (http://atomicrobotdesign.com/blog/htmlcss/build-a-vertical-scrolling-shooter-game-with-html5-canvas-part-1).

The tutorial is divided into 12 steps. A copy of the code at each step is available in the tutorial directory. If you want to skip the explanation and go it alone, the finished game is located in index.html.

This tutorial is not intended to be a beginner's introduction to JavaScript. Instead, it is an introduction to some of the principles of game design. It assumes a basic understanding of HTML, as well as a grasp of the fundamentals of JavaScript (such as the role of variables, functions and objects). If you've learned a bit of JavaScript, but don't know what to do with your new knowledge, this is for you.

### Step One - Basic HTML

This is a minimal HTML5 document:
```
...
<!DOCTYPE html>
<html>
<head>
</head>
<body>
</body>
</html>
...
```

To the body of this document, we'll add a canvas element. This is where the game will actually be drawn. Giving it an id will allow us to easily access it with JavaScript.
```
...
<body>
	<canvas id="canvas"></canvas>
</body>
...
```

To the head section, we'll add some CSS styles to make the body and canvas visible. CSS will not be a major factor in this tutorial. Feel free to continue even if this is gibberish.
```
...
<head>
<style>
body {
	background: gray;
}
canvas {
	display: block;
	margin: 30px auto 0;
	border: 1px dashed white;
	background: black;
}
</style>
</head>
...
```

If you save this file with a .html extension and open it with a web browser, you should see a black box with a white border in the middle of a gray page.

A title is a nice touch.
```
...
<head>
<title>Pew-Pew</title>
<style>
...
```

We'll need a place to put our JavaScript also.
```
...
<style>
<script>
</script>
</head>
...
```

Finally, it's good practice to include language and character set information.
```
...
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Pew-Pew</title>
...
```

If you've been following along, your file should now be identical to tut01.html in the tutorial directory.

### Step Two - Initialization

Any time we load our game, we'll want to do some setup. Before we can display anything to the screen, we'll need to access the canvas element. Let's create some canvas variables.
```
...
<script>
//canvas vars
var canvas;
var CANVAS_W = 600;
var CANVAS_H = 600;
var ctx;
</script>
...
```

We'll connect our canvas variable to the canvas element, which will allow JavaScript to access it, and we'll store a width and height for the canvas. These should never change during our program. Variables that do not change are called "constants" and are often capitalized to remind us not to change them. Finally, ctx is an abbreviation for context. Every canvas element has a context, and this is where drawing actually takes place.

When we start our game, we'll want to tie our canvas variables to the HTML canvas element and apply the width and height. To do so, we'll create an init function.
```
...
var ctx;

//init functions
function init() {
	canvas = document.getElementById('canvas');
	canvas.width = CANVAS_W;
	canvas.height = CANVAS_H;
	ctx = canvas.getContext('2d');
}
</script>
...
```
Whenever we call init, it will store the canvas HTML element in the canvas variable, set the width and height, and store the context in the ctx variable.

Finally, let's call init when the window has loaded and is ready to go.
```
...
}

//start game
window.onload = init;
</script>
...
```

When you load this file, you should see that the canvas has been resized according to CANVAS_W and CANVAS_H. Your code should now match tut02.html in the tutorial directory.

### Step Three - Draw the Ship

Now that we have a canvas, let's draw something. Similar to the canvas, we'll give our ship some parameters. The ship variable will hold an image object. Our location on the canvas will be tracked by x and y. I used the dimensions of my ship image for W and H. If you use something different, change them accordingly.
```
...
var ctx;

//ship vars
var ship;
var SHIP_W = 50;
var SHIP_H = 57;
var ship_x;
var ship_y;

//init functions
...
```

It's helpful to give the ship an init function too, as we'll want to reinitialize the ship whenever it is destroyed. These formulas will center the ship near the bottom of the screen.
```
...
}

function shipInit() {
	ship_x = (CANVAS_W / 2) - (SHIP_W / 2);
	ship_y = CANVAS_H - (1.5 * SHIP_H);
}
...
```

Our canvas context has a built-in drawImage method which will accept an image and a target coordinate. Let's wrap that up in a draw function.
```
...
}

//draw functions
function drawShip() {
	ctx.drawImage(ship, ship_x, ship_y);
}

//start game
...
```

Finally, our main init function will need to put this all together. We'll run our ship's init function, store a ship image in an Image object and finally, draw the ship.
```
...
	ctx = canvas.getContext('2d');
	shipInit();
	ship = new Image();
	ship.src = 'ship.png';
	drawShip();
}
...
```

We've now completed the third tutorial step. If you load this in your browser, you should see the ship at the bottom of the canvas. If you don't see it right away, try refreshing the page. It can take longer to load an image than to execute the rest of the code. This is an issue we'll resolve in later steps.

### Step Four - Game Loop

In this step, we'll create our game loop. The game loop runs continuously during the game, and will do all of the real work (like moving players, checking collisions and keeping the score up to date). We'll need one new variable.

```
...
var ship_y;

//misc vars
var FPS = 30;

//init functions
...
```
The FPS variable will determine how many time the game loop runs per second (Frames Per Second). For our purposes, 30 should be fast enough to give us smooth movements.

We'll need one new function (aside from the game loop itself).
```
...
//draw functions
function clearCanvas() {
	ctx.clearRect(0, 0, CANVAS_W, CANVAS_H);
}

function drawShip() {
...
```
This function will erase the canvas. Every time something in the game moves, we want to erase the old images before we draw them in their new locations.

For now, the game loop only needs to clear the screen and redraw the ship.
```
...
var FPS = 30;

//game loop
function gameLoop() {
	clearCanvas();
	drawShip();
}

//init functions
...
```

Lastly, we'll use the init function to start the game loop. Notice that drawShip has moved from init to gameLoop.
```
...
function init() {
    canvas = document.getElementById('canvas');
    canvas.width = CANVAS_W;
    canvas.height = CANVAS_H;
    ctx = canvas.getContext('2d');
    shipInit();
    ship = new Image();
    ship.src = 'ship.png';
    setInterval(gameLoop, 1000 / FPS);
}
...
```
The setInterval function calls a function ever so many thousands of a second. This code will call gameLoop 30 times each second.

That brings us to the end of step 4. If you load this in the browser, you won't actually see anything new happening, but behind the scenes, the canvas in repeatedly drawing and erasing your ship image.
