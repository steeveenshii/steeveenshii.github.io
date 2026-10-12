# Video Games

<p>
  <a href="https://steeveenshii.github.io/" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #267CB9; border-radius: 6px; font-weight: bold; text-decoration: none; margin-right: 8px;">← Return to Home</a>
  <a href="https://steeveenshii.github.io/cube-dash.html" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #39a852; border-radius: 6px; font-weight: bold; text-decoration: none;">Play Cube Dash →</a>
</p>

## Cube Dash

Cube Dash is a game I made with help from AI. It has two levels: **Stereo Happiness**, where the player jumps as a cube, and **Back on Course**, which adds a ship mode. The player can use the mouse, Space bar, or Up Arrow to jump. In ship mode, holding the control makes the ship fly upward.

## My learning goal

My goal is to get better at breaking down code so I can understand what each part does instead of only using it. I want to understand how the game works, especially how it tracks progress, detects crashes, and knows when a level is complete.

## How the game tracks progress

The game keeps a number called `scroll`. Every frame, the game adds the speed to `scroll`, so the level moves forward. Then it divides `scroll` by the total length of the level to calculate a percentage for the progress bar.

~~~javascript
function progress(){
  const percent = Math.min(100, Math.floor(scroll / level.len * 100));
  bar.style.width = percent + '%';
  pct.textContent = percent + '%';
  return percent;
}
~~~

When `scroll` reaches `level.len`, the game calls the function that shows the level-complete screen. I am learning to trace values like `scroll`, `level.len`, and `percent` step by step to understand how the code works.

## Controls

- **Click, Space, or Up Arrow:** jump as the cube.
- **Hold the control in ship mode:** fly upward.
- **Escape:** pause the game.

<p>
  <a href="https://steeveenshii.github.io/" style="display: inline-block; padding: 12px 18px; color: #ffffff; background: #267CB9; border-radius: 6px; font-weight: bold; text-decoration: none;">← Return to Home</a>
</p>
