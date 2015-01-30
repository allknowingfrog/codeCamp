## Pew-Pew - An HTML5, Vertical Scrolling Shooter

This tutorial series was originally presented at Sycamore Code Camp 2014. It is inspired by Mike Thomas's excellent tutorial (http://atomicrobotdesign.com/blog/htmlcss/build-a-vertical-scrolling-shooter-game-with-html5-canvas-part-1).

The tutorial is divided into 12 steps. A copy of the code at each step is available in the tutorial directory. If you want to skip the explanation and go it alone, the finished game is located in index.html.

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
This tutorial will assume some basic knowledge of HTML. If any part of that document puzzles you, you should go find some basic knowledge of HTML.

To the body of this document, we'll add a canvas element. This is where the game will actually be rendered. Giving it an id will allow us to easily access it with JavaScript.
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

Comments in JavaScript are defined by two slashes (//). Anything after is ignored. After the comment, we create a variable to hold the canvas. This will allow JavaScript to access it. We'll store a width and height for the canvas. These should never change during our program. Variables that do not change are called "constants" and are often capitalized to remind us not to change them. Finally, ctx is an abbreviation for context. Every canvas element has a context, and this is where drawing actually takes place.

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
Whenever we call init, this code will execute. It will store the canvas HTML element in the canvas variable, set the width and height, and store the context in the ctx variable.

Finally, let's call our function when the window has loaded and is ready to go.
```
...
}

//start game
window.onload = init;
</script>
...
```

When you load this file, you should see that the canvas has been resized according to CANVAS_W and CANVAS_H. Your code should now match tut02.html in the tutorial directory.
