---
layout: default
title: Cube Dash
---

# Cube Dash

<p>
  <a href="https://steeveenshii.github.io/" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #267CB9; border-radius: 6px; font-weight: bold; text-decoration: none; margin-right: 8px;">← Return to Home</a>
  <a href="https://steeveenshii.github.io/cube-dash-v13.html" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #39a852; border-radius: 6px; font-weight: bold; text-decoration: none;">Play Cube Dash v13 →</a></p>

# Innovator's Journal: Cube Dash — v1 to v13

## Process and outcome

In class, I used WebbGPT to help develop **Cube Dash**. I described the kind of game I wanted, then tested the result and looked through the code to understand how the game works. The newest version has 4 playable levels: **Stereo Happiness**, **Back on Course**, **Sunshine**, and **Grief**. It also includes cube, ship, and wave movement, a progress bar, attempts, collision detection, and collectible coins.

My outcome is a playable game that I can test in the browser. I am not only using AI for the game; I am learning to read the code one part at a time and connect each code section to something I can see happen in the game.

## Version comparison: Cube Dash v1 and v13

This comparison only uses my first version and my newest version. Version 1 proved that the basic game idea worked. Version 13 shows how I improved it by adding and testing more systems.

| Feature | Cube Dash v1 | Cube Dash v13 |
| --- | --- | --- |
| Levels | One level: **Stereo Happiness** | 4 playable levels: **Stereo Happiness**, **Back on Course**, **Sunshine**, and **Grief** |
| Movement | Cube jumping | Cube, ship, and wave movement |
| Challenges | Spikes, blocks, and platforms | More obstacles, portals, coins, and harder sections |
| Player feedback | Menu, attempts, and a progress bar | Selectable levels, progress, attempts, coins, and more visual feedback |
| What I learned | The basic game loop, jumping, and collision checks | How connected systems, testing, and clearer AI questions can make a game more complete |

The biggest change is that v1 was one playable level, while v13 is a more complete game with choices and different ways to move. I learned that software development happens through small improvements: make one feature work, test it, understand the code, and then build on it.

### Cube Dash v1: my starting point

This first version has one playable level. It helped me understand the basic connection between jumping, obstacles, collision checks, attempts, and progress.

<iframe src="https://steeveenshii.github.io/cube-dash-v1.html" title="Cube Dash v1 game" width="900" height="500" style="width:100%; max-width:900px; border:2px solid #7b61ff; border-radius:8px;"></iframe>


## AI Code Map: how AI helped me understand Cube Dash

I used WebbGPT to help create the first version of Cube Dash, but I also used AI as a guide while I studied the code. Instead of asking AI to do everything for me, I asked it where important parts of the game are and what they do. This helped me break a large program into smaller parts that I could test and understand.

## Development process: how I used AI and guided the results

### From an idea to a playable game

I started with the idea of making a fast obstacle game that I could play in the browser. I used WebbGPT to help create a first version of **Cube Dash**, then I tried the result myself instead of treating the first answer as finished. I decided what kind of game I wanted, played each version, and checked whether the movement, obstacles, levels, and progress felt like the game I had in mind.

The first working version gave me a large amount of code, so I could not understand everything at once. I used AI to make the project easier to learn from. I asked it to help me locate important sections of the code, such as the canvas, controls, repeating game loop, collision checks, win condition, and progress tracking. Then I read the nearby lines and connected them to something I could see happen while playing.

### How my questions became more specific

At first, my AI request was broad: help me make a game. After I had a playable result, my questions became more focused because I had real things to test and investigate. For example, I asked questions like:

- Where does Cube Dash remember the player position, speed, and attempts?
- Which code runs repeatedly while the game is playing?
- What changes when I press Space or the Up Arrow?
- Where is the comparison that makes the player lose after hitting an obstacle?
- How does the game decide that I finished a level?

These are examples of the kinds of questions I used to guide AI; they helped me turn one big file into smaller questions I could answer. I did not just copy an answer. After AI pointed me to a location, I looked at the code, played the game again, and checked whether the explanation matched the result on screen.

### My testing and improvement cycle

My process was: **play the game → notice a behavior or question → find the related code with AI → read it → test again.** For example, when I wanted to understand progress, I looked for the value that changes as the player moves through the level. I learned that **scrollX** is used to move through the level, and the game wins when **scrollX** reaches the level length. When I wanted to understand losing, I followed **rectsOverlap(a, b)** and **die()** and then watched what happened when the cube hit a spike.

This made AI useful as a learning tool instead of a shortcut. A success was getting a playable game with **4 playable levels**, different movement modes, obstacles, coins, attempts, and a progress bar. A challenge was that the code looked confusing at first because there was so much of it. I handled that challenge by studying one system at a time instead of trying to understand the whole game in one reading.

## What I learned about software and coding

I deepened my understanding of how a game is organized. The **state variables** keep track of changing information such as the player, attempts, **scrollX**, the current level, and whether the player is holding jump. The **game loop** keeps calling **update()** and **draw()** with **requestAnimationFrame(loop)**, so the game can calculate changes and then show them every frame. The **input events** change **holdJump**, and the update code uses that value to decide how the cube, ship, or wave moves.

I also learned that a game does not need one normal score number to show progress. Cube Dash tracks progress using the player's movement through the level, and it keeps track of attempts and collectible coins. I understand more clearly how an if statement can create a rule: if the player's rectangle overlaps an obstacle, the game calls **die()**; if **scrollX** reaches the level length, the game calls **win()**.

The biggest lesson is that software is built from connected pieces. A small variable or condition can change what the player sees on the screen. AI helped me find those pieces faster, but testing and reading the code helped me understand why they work.

## Reflection and my future approach

I completed this project by keeping momentum through short cycles instead of waiting until I understood every line. I moved from an idea, to a first prototype, to testing, to documenting what the code does. Next time, I will begin with a short design plan, make one feature work at a time, and test after each change. I will use AI to help brainstorm, explain code, and point me to relevant sections, but I will still make the final decisions and verify every change in the game.

For future software projects, I will keep a small development log with the feature I worked on, the question I asked AI, the code area I changed or studied, and the result of my test. This will help me catch mistakes, explain my choices, and improve the game step by step.


| What I wanted to find | Where it is in the code | What I learned |
| --- | --- | --- |
| The drawing area | `<canvas id="game" width="900" height="500"></canvas>` near the top of the HTML | The canvas is the screen where the game draws the player, obstacles, and background. |
| The repeating game code | `function loop(){ update(); draw(); requestAnimationFrame(loop); }` | The game keeps running because it updates the information and redraws the screen over and over. |
| The controls | the `keydown`, `keyup`, mouse, and touch event listeners | The Space bar and Up Arrow change `holdJump`, which tells the player when to jump or fly. |
| The lose part | `rectsOverlap(a, b)` and the calls to `die()` | The game compares the player's rectangle to spikes and blocks. If they overlap, `die()` starts the lose screen. |
| The win part | `if(scrollX >= L.len){ win(); return; }` | The game wins when `scrollX`, the distance travelled through the level, reaches `L.len`, the level length. |
| The progress display | `scrollX / L.len * 100` | The same distance variable is used to calculate the percentage in the progress bar. |

AI helped me find these locations faster, but I still read the nearby lines and tested the game to make sure I understood what was happening. My next step is to explain one part of the code without AI, then make a small change and test what happens.


## Game documentation

The live game below shows the menu and **four playable levels**. It is my visual documentation of the game that WebbGPT helped me develop.

<iframe src="https://steeveenshii.github.io/cube-dash-v13.html" title="Cube Dash v13 game" width="900" height="500" style="width:100%; max-width:900px; border:2px solid #267CB9; border-radius:8px;"></iframe>

## The canvas

The canvas is in the HTML near the top of the page:

~~~html
<canvas id="game" width="900" height="500"></canvas>
~~~

It is the drawing area for the game. In JavaScript, the game finds it with <code>document.getElementById('game')</code> and gets a 2D drawing context with <code>getContext('2d')</code>. Its size is **900 pixels wide by 500 pixels high**. If I change the width or height, the drawing space changes too. For example, a wider canvas would show more of the level at one time, while a smaller height would make the ground and ceiling feel closer together. I would also need to check values such as <code>GROUND_Y = 420</code> so the level still fits the new canvas size.

## The state variables

The game keeps track of important information in variables declared near the start of the script:

~~~javascript
let state = 'menu';
let scrollX = 0;
let attempts = 1;
let holdJump = false;
let deadTimer = 0;
let particles = [];
let waveTrail = [];
let tick = 0;
let curLevel = 0;

const player = { x:150, y:GROUND_Y - CUBE, vy:0, onGround:true, rot:0, mode:'cube' };
~~~

<code>state</code> tracks whether the game is on the menu, playing, paused, dead, or won. <code>scrollX</code> tracks how far the level has moved. <code>attempts</code> counts retries. The <code>player</code> object keeps the player position (<code>x</code> and <code>y</code>), vertical speed (<code>vy</code>), whether it is on the ground, its rotation, and its current mode: cube, ship, or wave. The arrays store temporary particles and the trail behind the wave.

## The game loop

The function that repeats is <code>loop()</code>:

~~~javascript
function loop(){
  update();
  draw();
  requestAnimationFrame(loop);
}
~~~

<code>requestAnimationFrame(loop)</code> asks the browser to run the loop again before the next screen refresh. Each time, <code>update()</code> changes the game data, such as movement, progress, and collisions. Then <code>draw()</code> redraws the background, obstacles, player, coins, and effects on the canvas.

## Input handling

The Space bar and Up Arrow control the game. The mouse and touch controls also call the same functions. When Space or Up Arrow is pressed, the game runs <code>press(e)</code>, which sets <code>holdJump = true</code>. When the key is released, <code>release()</code> sets it back to <code>false</code>.

~~~javascript
if(e.code==='Space'||e.code==='ArrowUp'){ press(e); }

function press(e){
  if(e) e.preventDefault();
  if(state==='paused') return;
  holdJump = true;
}
~~~

The game checks <code>holdJump</code> during <code>update()</code>. As a cube, it uses the value to jump when the player is on the ground. As a ship or wave, holding the control makes the player travel upward. Escape calls <code>togglePause()</code>.

## Collision and win-lose logic

The game uses a function called <code>rectsOverlap(a, b)</code> to compare two rectangles. It returns true when the player and another object overlap:

~~~javascript
function rectsOverlap(a,b){
  return a.x < b.x+b.w && a.x+a.w > b.x &&
         a.y < b.y+b.h && a.y+a.h > b.y;
}
~~~

The game compares the player rectangle to blocks and spike rectangles. If there is an overlap with a dangerous object, it runs <code>die()</code>. The game wins when the scrolling distance reaches the length of the current level:

~~~javascript
if(scrollX >= L.len){ win(); return; }
~~~

This helped me understand that a game can check collisions using numbers for x-position, y-position, width, and height instead of needing a special collision tool.

## Progress, attempts, and coins

The game does not have one regular number called score. Instead, it tracks **progress**, **attempts**, and **coins**. Progress is calculated from the level distance:

~~~javascript
const pct = Math.min(100, Math.floor(scrollX/L.len*100));
progressInner.style.width = Math.min(100, scrollX/L.len*100) + '%';
~~~

<code>scrollX</code> grows by <code>SPEED</code> during each update. Dividing it by <code>L.len</code> creates the percentage shown in the progress bar. <code>attempts</code> increases in the retry button code. Coins are stored in <code>L.coins</code>; when the player reaches a coin, the code changes <code>c.taken</code> from false to true and updates the coin display.

## What I noticed and learned

I noticed that one small variable can affect many parts of the game. For example, <code>scrollX</code> moves the obstacles on screen, changes the progress percentage, and tells the game when the level is finished. I also learned that the same input variable, <code>holdJump</code>, can create different movement because the code checks the player's current mode. Breaking the code into canvas, variables, input, update, and draw sections makes a big program easier to understand.

## Goals for this week

1. **Trace one full game frame:** I will follow <code>loop()</code>, <code>update()</code>, and <code>draw()</code> in order and write a short note about what changes in each function.
2. **Test the canvas size:** I will change the canvas to two different sizes and record what happens to the player, ground, obstacles, and layout. Then I will fix at least one value, such as <code>GROUND_Y</code>, to fit the new size.
3. **Understand collision math:** I will use the <code>rectsOverlap()</code> function to explain in my own words what each x, y, width, and height comparison checks.
4. **Create player accounts and unlockables:** I will create an account system that remembers how many coins a player has collected and which colors they have unlocked. I will test that the information is still saved after the player leaves and returns to the game.

<p>
  <a href="https://steeveenshii.github.io/" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #267CB9; border-radius: 6px; font-weight: bold; text-decoration: none;">← Return to Home</a>
</p>
