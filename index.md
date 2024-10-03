---
title: Welcome to Andrew Cai's GitHub Website!
---

<p style="margin-bottom:2em;white-space:pre"> </p>

<!-- Reminder for editing this webpage: please make my webpages read-only to prevent hacker attacks, because the user is likely to already have access to good social media platforms, and for this webpage, I do not want to use more third-party online platforms so far -->

<img src="https://avatars.githubusercontent.com/u/181604248?s=400&u=88a726ffaab91d761b32cb516b31b18dad0c521c&v=4" alt="Image of Andrew Cai" width="350" height="350" style="border:12px solid brown;border-style:groove;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">

<p style="margin-bottom:2em;white-space:pre"> </p>

<h3>Current Goals:</h3>
- Earn a Bachelor's Degree in Computer Science from San Francisco State University in the Spring 2028 semester
- Earn a useful job that utilizes my understanding of computer science
- Retire comfortably 👴

<p style="margin-bottom:2em;white-space:pre"> </p>

<iframe src="https://www.google.com/maps/embed?pb=!1m10!1m8!1m3!1d12622.913523266852!2d-122.481876!3d37.72605300000001!3m2!1i1024!2i768!4f13.1!5e0!3m2!1sen!2sus!4v1727420687329!5m2!1sen!2sus" width="600" height="450" style="border:12px solid brown;border-style:groove;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>

<p style="margin-bottom:2em;white-space:pre"> </p>

<p style="vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto;width:75%;text-align:center">
      👨‍🎓 On the afternoon of September 14th, 2024, this webpage was created at San Francisco State University in West Grove Commons by using the webpage at the URL, "<a href="https://github.com/skills/github-pages">https://github.com/skills/github-pages</a>".
</p>

<p style="margin-bottom:2em;white-space:pre"> </p>

<hr>

<p style="margin-bottom:1em;white-space:pre"> </p>

<details style="border:12px solid azure;border-radius:5px;border-style:groove;background-color:lightblue">
      <summary style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            <p style="margin-bottom:1em;white-space:pre"> </p>
            Game about a Snake
            <p style="margin-bottom:1em;white-space:pre"> </p>
      </summary>
      
      <h3 style="vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto;width:50%;text-align:center">Andrew Cai's Snake Game Program:</h3>
      
      <p style="margin-bottom:2em;white-space:pre"> </p>
      
      <canvas id="canvas" width="500" height="500" style="border:3px solid brown;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto"></canvas>
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
              ctx.fillStyle = "aquamarine";
              ctx.fillRect(0, 0, width, height);
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
              drawBorder();
              drawScore();
              snake.move();
              snake.draw();
              apple.draw();
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
      
      <p style="margin-bottom:2em;white-space:pre"> </p>
      
      <details style="border:3px solid green;border-radius:5px;border-style:groove;background-color:aquamarine">
      
            <summary style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            <p style="margin-bottom:1em;white-space:pre"> </p>
            🧑‍💻 Details of the program shown above
            <p style="margin-bottom:1em;white-space:pre"> </p>
            </summary>
            
            <p style="margin-bottom:1em;white-space:pre"> </p>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
                  On the evening of September 14th, 2024, at San Francisco State University in West Grove Commons, I wanted to try to evaluate GitHub Pages, so I created the version of a Snake game program directly above this paragraph. The version of a Snake game program directly above this paragraph is from the first three pages in the "Putting It All Together" section of Chapter 17 in Part III of the book, <i>JavaScript for Kids: A Playful Introduction to Programming</i>.
            </p>
            
            <br>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            The author of the book is Nick Morgan. Additionally, the illustrator of the book is Miran Lipovača. In addition, the technical reviewer of this book is Angus Croll.
            </p>
            
            <br>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            Also, William Pollock is the publisher of this book. Additionally, the production editor of this book is Riley Hoffman. In addition, the developmental editors of this book are William Pollock and Seph Kramer.
            </p>
            
            <br>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            Also, the copyeditor of this book is Rachel Monaghan. Additionally, Tina Salameh created the cover illustration of this book. In addition, the compositor of this book is Riley Hoffman.
            </p>
            
            <br>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            🧑‍💼 Also, the proofreader of this book is Paula L. Fleming. Additionally, No Starch Press published this book. In addition, the publication date of this book is December 14, 2014.
            </p>
            
            <br>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">To control the snake's movement, press the W, A, S, and D keys on a computer keyboard:</p>
            <ul style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
                  <li>The "W" key: Moves the snake upward</li>
                  <li>The "A" key: Moves the snake to the left</li>
                  <li>The "S" key: Moves the snake downward</li>
                  <li>The "D" key: Moves the snake to the right</li>
            </ul>
            
            <br>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">To restart the game, please reload the webpage.</p>
            <p style="margin-bottom:1em;white-space:pre"> </p>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">JavaScript Code Sample:</p>
            <pre><code style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">console.log("Hello, world!");</code></pre>
      </details>
      
      <p style="margin-bottom:2em;white-space:pre"> </p> 

</details>

<p style="margin-bottom:1em;white-space:pre"> </p>

<hr style="border:3px solid green;border-radius:5px;border-style:groove">

<p style="margin-bottom:2em;white-space:pre"> </p>

<details style="border:12px solid azure;border-radius:5px;border-style:groove;background-color:lightblue">
      <summary style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
            <p style="margin-bottom:1em;white-space:pre"> </p>
            Video about Google 🧑‍💻
            <p style="margin-bottom:1em;white-space:pre"> </p>
      </summary>
      
      <h3 style="vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto;width:75%;text-align:center">🧑‍💻 "Google — 25 Years in Search: The Most Searched"</h3>
      
      <p style="margin-bottom:1em;white-space:pre"> </p>
      
      <iframe src="https://www.youtube.com/embed/3KtWfp0UopM" width="350" height="350" title="YouTube video titled 'Google — 25 Years in Search: The Most Searched'" style="border:6px solid brown;border-style:groove;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto"></iframe>
      
      <p style="margin-bottom:1em;white-space:pre"> </p>
      
      <details style="border:3px solid green;border-radius:5px;border-style:groove;background-color:aquamarine">
      
            <summary style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
                  <p style="margin-bottom:1em;white-space:pre"> </p>
                  🧑‍💻 Details of the video shown above
                  <p style="margin-bottom:1em;white-space:pre"> </p>
            </summary>
            
            <p style="margin-bottom:1em;white-space:pre"> </p>
            
            <p style="width:600px;vertical-align:middle;display:block;float:none;left:0;right:0;margin:auto">
                  The video was published on Dec 11, 2023. <br>
                  "Google is celebrating the most searched figures and moments in 25 years of Google Search. From BTS to Taylor Swift, see the moments that have changed the world and inspired the next generation to come. Explore more at <a href="https://trends.google.com/trends/yis/2023/GLOBAL/">https://trends.google.com/trends/yis/2023/GLOBAL/</a> <br>
                  
                  #YearinSearch <br>
                  
                  Audio described version here: <a href="https://www.youtube.com/watch?v=3KtWfp0UopM">https://www.youtube.com/watch?v=3KtWfp0UopM</a> <br>
                  
                  Music by: <br>
                  U2 'I Still Haven’t Found What I’m Looking For' <br>
                  Beyoncé 'Crazy in Love' <br>
                  Taylor Swift 'Wildest Dreams (Taylor's Version)' <br>
                  Tina Turner 'The Best' <br>
                  Composers: Marcel Neumann, Simon Heeger, Christian Vorländer, Hermann Schepetkov <br>
                  Additional Vocals: United Voices Chicago, Hugh O‘Neil <br>
                  Music Producer: Lukas Lehmann <br>
                  Executive Music Producer: Simon Heeger <br>
                  Music production company: 2WEI Music <br>
                  Sfx & Mix: Marcel Neumann" <br>
                  
                  🧑‍🎨 <br>
                  
                  "Footage courtesy of: <br>
                  Major League Baseball footage used with permission of Major League Baseball. All rights reserved. <br>
                  SPIDER-MAN: INTO THE SPIDER-VERSE © 2018 Sony Pictures Animation Inc. All Rights Reserved. Courtesy of Sony Pictures Animation <br>
                  MARVEL and all related character names: © & TM 2023 MARVEL <br>
                  'BARBIE®' and associated trademarks and trade dress are owned by and used with permission of Mattel, Inc. © 2023 Mattel, Inc. All Rights Reserved. <br>
                  Footage provided by Veritone <br>
                  Getty Images <br>
                  BBC Motion Gallery/Getty Images <br>
                  NASA <br>
                  Max Headroom CREATED BY ROCKY MORTON, ANNABEL JANKEL AND GEORGE STONE <br>
                  Scripps National Spelling Bee, all rights reserved <br>
                  The White Helmets <br>
                  Associated Press <br>
                  KGO-TV/ABC7 <br>
                  The Footage Company <br>
                  © 2023 Viacom Media Networks, a division of Viacom International Inc. All Rights Reserved <br>
                  © Henri Calderon for Colossal <br>
                  French Tennis Federation <br>
                  USTA <br>
                  CBS News      "🧑‍💼" <br>
                  NCAA <br>
                  Thanks to © Red Bull Media House <br>
                  Dick Clark Productions <br>
                  Courtesy to FIFA <br>
                  Footage provided by IMG Archive <br>
                  The Pokémon Company International <br>
                  TB12 Sports <br>
                  Disney+ <br>
                  Gracie Films <br>
                  Warner Brothers" 🧑‍🎨 <br>
            </p>
            <p style="margin-bottom:1em;white-space:pre"> </p>
      </details>

</details>

<p style="margin-bottom:2em;white-space:pre"> </p>

<hr style="border:3px solid green;border-radius:5px;border-style:groove">

<p style="margin-bottom:2em;white-space:pre"> </p>
