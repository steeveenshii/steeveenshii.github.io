---
layout: default
title: Video Games
---

# Video Games

<p>
  <a href="https://steeveenshii.github.io/" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #267CB9; border-radius: 6px; font-weight: bold; text-decoration: none; margin-right: 8px;">← Return to Home</a>
  <a href="https://steeveenshii.github.io/cube-dash-v4.html" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #39a852; border-radius: 6px; font-weight: bold; text-decoration: none;">Play Cube Dash v4 →</a>
</p>

# Innovator's Journal: Cube Dash v4

## Process and outcome

In class, I used WebbGPT to help develop **Cube Dash**. I described the kind of game I wanted, then tested the result and looked through the code to understand how the game works. The new version has three levels: **Stereo Happiness**, **Back on Course**, and **Grief**. It also includes cube, ship, and wave movement, a progress bar, attempts, collision detection, and collectible coins.

My outcome is a playable game that I can test in the browser. I am not only using AI for the game; I am learning to read the code one part at a time and connect each code section to something I can see happen in the game.

## AI Code Map: how AI helped me understand Cube Dash

I used WebbGPT to help create the first version of Cube Dash, but I also used AI as a guide while I studied the code. Instead of asking AI to do everything for me, I asked it where important parts of the game are and what they do. This helped me break a large program into smaller parts that I could test and understand.

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

The live game below shows the menu and the three selectable levels. It is my visual documentation of the game that WebbGPT helped me develop.

<iframe src="https://steeveenshii.github.io/cube-dash-v4.html" title="Cube Dash v4 game" width="900" height="500" style="width:100%; max-width:900px; border:2px solid #267CB9; border-radius:8px;"></iframe>

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
4. **Make one meaningful game change:** I will add or adjust one obstacle, portal, or coin in a level and test it at least three times to make sure it works.

<p>
  <a href="https://steeveenshii.github.io/" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #267CB9; border-radius: 6px; font-weight: bold; text-decoration: none;">← Return to Home</a>
</p>
