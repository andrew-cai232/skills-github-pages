---
title: Welcome to Andrew Cai's GitHub blog!
---

<!-- Reminder for editing this webpage: please make my webpages read-only to prevent hacker attacks, because the user is likely to already have access to good social media platforms, and for this webpage, I do not want to use more third-party online platforms so far -->

Image Test:

<img src="https://avatars.githubusercontent.com/u/181604248?s=400&u=88a726ffaab91d761b32cb516b31b18dad0c521c&v=4" alt="Image of Andrew Cai" width="350" height="350" style="border:3px solid brown;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">

This webpage was created at San Francisco State University in West Grove Commons. Also, this webpage was created on the afternoon of September 14th, 2024.
I created this webpage to create "a site or blog from" my "GitHub repositories with GitHub Pages". I created this webpage by using the webpage at the URL, "[https://skills.github.com](https://github.com/skills/github-pages)".

<hr>

Andrew Cai's version of a Snake game program:

<canvas id="canvas" width="500" height="500"></canvas>
<script src="https://code.jquery.com/jquery-2.1.0.js"></script>
<script>
      //	Set	up	canvas
      var canvas = document.getElementById("canvas");
      var ctx = canvas.getContext("2d");
      //	Get	the	width	and	height	from	the	canvas	element
      var width = canvas.width;
      var height = canvas.height;
      //	Work	out	the	width	and	height	in	blocks
      var blockSize = 10;
      var widthInBlocks = width / blockSize;
      var heightInBlocks = height / blockSize;
      //	Set	score	to	0
      var score = 0;
      //	Draw	the	border
      var drawBorder = function() {
        ctx.fillStyle = "Gray";
        ctx.fillRect(0, 0, width, blockSize);
        ctx.fillRect(0, height - blockSize, width, blockSize);
        ctx.fillRect(0, 0, blockSize, height);
        ctx.fillRect(width - blockSize, 0, blockSize, height);
      };
      //	Draw	the	score	in	the	top-left	corner
      var drawScore = function() {
        ctx.font = "20px	Courier";
        ctx.fillStyle = "Black";
        ctx.textAlign = "left";
        ctx.textBaseline = "top";
        ctx.fillText("Score:	" + score, blockSize, blockSize);
      };
      //	Clear	the	interval	and	display	Game	Over	text
      var gameOver = function() {
        clearInterval(intervalId);
        ctx.font = "60px	Courier";
        ctx.fillStyle = "Black";
        ctx.textAlign = "center";
        ctx.textBaseline = "middle";
        ctx.fillText("Game	Over", width / 2, height / 2);
      };
      //	Draw	a	circle	(using	the	function	from	Chapter	14)
      var circle = function(x, y, radius, fillCircle) {
        ctx.beginPath();
        ctx.arc(x, y, radius, 0, Math.PI * 2, false);
        if (fillCircle) {
          ctx.fill();
        } else {
          ctx.stroke();
        }
      };
      //	The	Block	constructor
      var Block = function(col, row) {
        this.col = col;
        this.row = row;
      };
      //	Draw	a	square	at	the	block's	location
      Block.prototype.drawSquare = function(color) {
        var x = this.col * blockSize;
        var y = this.row * blockSize;
        ctx.fillStyle = color;
        ctx.fillRect(x, y, blockSize, blockSize);
      };
      //	Draw	a	circle	at	the	block's	location
      Block.prototype.drawCircle = function(color) {
        var centerX = this.col * blockSize + blockSize / 2;
        var centerY = this.row * blockSize + blockSize / 2;
        ctx.fillStyle = color;
        circle(centerX, centerY, blockSize / 2, true);
      };
      //	Check	if	this	block	is	in	the	same	location	as	another	block
      Block.prototype.equal = function(otherBlock) {
        return this.col === otherBlock.col && this.row === otherBlock.row;
      };
      //	The	Snake	constructor
      var Snake = function() {
        this.segments = [
          new Block(7, 5),
          new Block(6, 5),
          new Block(5, 5)
        ];
        this.direction = "right";
        this.nextDirection = "right";
      };
      //	Draw	a	square	for	each	segment	of	the	snake's	body
      Snake.prototype.draw = function() {
        this.segments[0].drawSquare("Green");
        for (var i = 1; i < this.segments.length; i++) {
          if (i % 2 === 1) {
            this.segments[i].drawSquare("Blue");
          } else if (i % 2 === 0) {
            this.segments[i].drawSquare("Yellow");
          }
        }
      };
      //	Create	a	new	head	and	add	it	to	the	beginning	of
      //	the	snake	to	move	the	snake	in	its	current	direction
      Snake.prototype.move = function() {
        var head = this.segments[0];
        var newHead;
        this.direction = this.nextDirection;
        if (this.direction === "right") {
          newHead = new Block(head.col + 1, head.row);
        } else if (this.direction === "down") {
          newHead = new Block(head.col, head.row + 1);
        } else if (this.direction === "left") {
          newHead = new Block(head.col - 1, head.row);
        } else if (this.direction === "up") {
          newHead = new Block(head.col, head.row - 1);
        }
        if (this.checkCollision(newHead)) {
          gameOver();
          return;
        }
        this.segments.unshift(newHead);
        if (newHead.equal(apple.position)) {
          score++;
          for (var i = 0; i < this.segments.length; i++) {
            while (apple.position.equal(this.segments[i])) {
              apple.move();
            }
          }
        } else {
          this.segments.pop();
        }
      };
      //	Check	if	the	snake's	new	head	has	collided	with	the	wall	or	itself
      Snake.prototype.checkCollision = function(head) {
        var leftCollision = (head.col === 0);
        var topCollision = (head.row === 0);
        var rightCollision = (head.col === widthInBlocks - 1);
        var bottomCollision = (head.row === heightInBlocks - 1);
        var wallCollision = leftCollision || topCollision ||
          rightCollision || bottomCollision;
        var selfCollision = false;
        for (var i = 0; i < this.segments.length; i++) {
          if (head.equal(this.segments[i])) {
            selfCollision = true;
          }
        }
        return wallCollision || selfCollision;
      };
      //	Set	the	snake's	next	direction	based	on	the	keyboard
      Snake.prototype.setDirection = function(newDirection) {
        if (this.direction === "up" && newDirection === "down") {
          return;
        } else if (this.direction === "right" && newDirection === "left") {
          return;
        } else if (this.direction === "down" && newDirection === "up") {
          return;
        } else if (this.direction === "left" && newDirection === "right") {
          return;
        }
        this.nextDirection = newDirection;
      };
      //	The	Apple	constructor
      var Apple = function() {
        this.position = new Block(10, 10);
      };
      //	Draw	a	circle	at	the	apple's	location
      Apple.prototype.draw = function() {
        this.position.drawCircle("LimeGreen");
      };
      //	Move	the	apple	to	a	new	random	location
      Apple.prototype.move = function() {
        var randomCol = Math.floor(Math.random() * (widthInBlocks - 2)) + 1;
        var randomRow = Math.floor(Math.random() * (heightInBlocks - 2)) + 1;
        this.position = new Block(randomCol, randomRow);
      };
      //	Create	the	snake	and	apple	objects
      var snake = new Snake();
      var apple = new Apple();
      //	Pass	an	animation	function	to	setInterval
      var intervalId = setInterval(function() {
        ctx.clearRect(0, 0, width, height);
        drawScore();
        snake.move();
        snake.draw();
        apple.draw();
        drawBorder();
      }, 100);
      //	Convert	keycodes	to	directions
      var directions = {
        65: "left",
        87: "up",
        68: "right",
        83: "down"
      };
      //	The	keydown	handler	for	handling	direction	key	presses
      $("body").keydown(function(event) {
        var newDirection = directions[event.keyCode];
        if (newDirection !== undefined) {
          snake.setDirection(newDirection);
        }
      });  
</script>

On the evening of September 14th, 2024, at San Francisco State University in West Grove Commons, I wanted to try to evaluate GitHub Pages, so I created the version of a Snake game program directly above this paragraph. The version of a Snake game program directly above this paragraph is from the first three pages in the "Putting It All Together" section of Chapter 17 in Part III of the book, _JavaScript for Kids: A Playful Introduction to Programming_.

The author of the book is Nick Morgan. Additionally, the illustrator of the book is Miran Lipovača. In addition, the technical reviewer of this book is Angus Croll.

Also, William Pollock is the publisher of this book. Additionally, the production editor of this book is Riley Hoffman. In addition, the developmental editors of this book are William Pollock and Seph Kramer.

Also, the copyeditor of this book is Rachel Monaghan. Additionally, Tina Salameh created the cover illustration of this book. In addition, the compositor of this book is Riley Hoffman.

Also, the proofreader of this book is Paula L. Fleming. Additionally, No Starch Press published this book. In addition, the publication date of this book is December 14, 2014.

JavaScript Code Sample:
``` javascript
console.log("Hello, world!");
```

<hr>

Current Task List:
- [ ] Earn a Bachelor's Degree in Computer Science from San Francisco State University in the Spring 2028 semester
- [ ] Earn a useful job that uses a moderate amount of ideas from computer science overall
- [ ] Retire successfully

<iframe src="https://github.com" width="350" height="350" title="GitHub website"></iframe>

Map Test:

```topojson
      {
            "type": "Topology",
            "transform": {
                  "scale": [0.0005000500050005, 0.00010001000100010001],
                  "translate": [100, 0]
            },
            "objects": {
                  "example": {
                        "type": "GeometryCollection",
                        "geometries": [
                        {
                              "type": "Point",
                              "properties": {"prop0": "value0"},
                              "coordinates": [4000, 5000]
                        },
                        {
                              "type": "LineString",
                              "properties": {"prop0": "value0", "prop1": 0},
                              "arcs": [0]
                        },
                        {
                              "type": "Polygon",
                              "properties": {"prop0": "value0",
                                    "prop1": {"this": "that"}
                              },
                              "arcs": [[1]]
                        }
                        ]
                  }
            },
            "arcs": [[[4000, 0], [1999, 9999], [2000, -9999], [2000, 9999]],[[0, 0], [0, 9999], [2000, 0], [0, -9999], [-2000, 0]]]
      }
```
