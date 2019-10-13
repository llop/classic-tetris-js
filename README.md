


classic-tetris-js
====================

An HTML5 + Javascript implementation of the popular game [Tetris]([https://en.wikipedia.org/wiki/Tetris](https://en.wikipedia.org/wiki/Tetris)), `classic-tetris-js` takes after the older versions of the game, such as [NES Tetris](https://tetris.wiki/Tetris_(NES,_Nintendo)).

Play `classic-tetris-js` [here](http://albertlobo.com/games/tetris)!

## Features

* Supported inputs: keyboard, mouse, touch and stylus pen.
* Starting level selection.
* 'Play', 'Pause' and 'Quit' functions.
* Additional game mechanics: [Ghost piece](https://tetris.fandom.com/wiki/Ghost_piece) and [Hard drop](https://tetris.fandom.com/wiki/Hard_Drop).
* Classic randomizer: 'Next' piece is randomly chosen with a slight bias against repeating pieces.
* Customizable graphics and sound.
* Powerful event system to notify in-game activity.

## Quick start

Options to add `classic-tetris-js` to your project:
* Install with [npm](https://npmjs.org): `npm install classic-tetris-js`
* [Download the latest release](https://github.com/llop/classic-tetris-js/archive/master.zip)
* Clone the repo: `git clone https://github.com/llop/classic-tetris-js.git`

## Basic setup

Include 'classic-tetris.js' in your HTML file.

```html
<script src='classic-tetris.js'></script>
``` 

Add the game's HTML elements:

```html
<canvas id='tetris-canvas' width='520' height='600'></canvas>
<div>
  <label for='level-input'>Start level:</label>
  <input id='level-input' name='level-input' type='number' min='0' max='19' value='5'></input>
  <button id='start-stop-btn'>Play/Pause</button>
  <button id='quit-btn'>Quit</button>
</div>
```

A `canvas` is required for the game to render. Besides that, it is convenient to also have a 'Start level' input, and buttons to play/pause and quit the game.

Finally, add the following script to initialize the game on window load:

```javascript
window.addEventListener('load', event => {
  const canvas = document.getElementById('tetris-canvas');
  const tetris = new ClassicTetris(canvas);
  document.getElementById('start-stop-btn').addEventListener('click', event => {
    const startLevel = document.getElementById('level-input').value;
    tetris.setStartLevel(startLevel);
    tetris.togglePlayPause();
  });
  document.getElementById('quit-btn').addEventListener('click', event => {
    tetris.quit();
  });
});
```

A working example can be found in [index.html](index.html).

## Game controls

The player can control the game using the keyboard. The following table shows the action-to-keys mapping:

| Action | Keys |
| --- | --- | 
| Move piece left | Left arrow, A |
| Move piece right | Right arrow, D |
| Move piece down | Down arrow, S |
| Rotate piece clockwise | Up arrow, W, X, K |
| Rotate piece anticlockwise | Z, L | 
| Hard drop | Space bar |
| Pause | ESC, P |

The player can also control the game using pointer devices such as a mouse, a touch screen, or a stylus pen:

| Action | Pointer gesture |
| --- | --- | 
| Move piece left | Move the pointer to the left of the piece | 
| Move piece right | Move the pointer to the right of the piece | 
| Move piece down | Place the pointer on the piece and move down | 
| Rotate piece clockwise | Click / tap (left mouse button, touch contact, pen contact),<br/>wheel up | 
| Rotate piece anticlockwise | Click / tap (mouse wheel, right mouse button, pen barrel button, X1 (back) mouse, X2 (forward) mouse, pen eraser button),<br/>wheel down | 

## Customizing the game

It is also possible to specify default values for many of the game's parameters. This can be accomplished via the `ClassicTetris` constructor, which takes an `options` object  after the required `canvas` element:

```javascript
const tetris = new ClassicTetris(canvas, {
   boardWidth: 10,
   // ... additional parameters here
 });
```

### Dimensions

The following parameters establish the size and location of the game board. Note that the `canvas` element will not be resized automatically, so its `width` and `height` attributes may need to be adjusted accordingly, as well the positioning of the [HUD elements](#heads-up-display). 

The recommended size for the `canvas` using default parameters is 520x600 pixels, as seen in the example above.

| Parameter name | Description | Default value |
| --- | --- | --- | 
| boardWidth | Width of the game board in terms of squares. | 10 | 
| boardHeight | Height of the game board in terms of squares. This value is the sum of:<br/> - Number of visible rows.<br/>- 2 invisible rows above the board, used to accomodate newly spawned pieces.  | 22 |
| boardX | Game board's left edge relative to the canvas's left edge (in pixels). | 30 |
| boardY | Game board's top edge relative to the canvas's top edge (in pixels).<br/>Note that since the board's top 2 rows are invisible, but taken into account for coordinates calculation in the render, this value may be negative. | -35 |
| squareSide | Side length of a square block (in pixels). | 28 |

### Heads-Up Display

The HUD displays information about the ongoing game: score, lines and next piece. The positioning of these elements, as well as the text's font and color, may be specified using the parameters below: 

| Parameter name | Description | Default value |
| --- | --- | --- | 
| canvasFont | Font properties for the canvas (see [HTML canvas  font  Property](https://www.w3schools.com/tags/canvas_font.asp) for more details). | '17px georgia' |
| canvasFontColor | Font color for the canvas (see [HTML canvas  fillStyle  Property](https://www.w3schools.com/tags/canvas_fillstyle.asp) for more details). | '#fff' |
| scoreX | Score label's left edge relative to the canvas's left edge (in pixels). | 330 |
| scoreY | Score label's top edge relative to the canvas's top edge (in pixels). | 100 |
| levelX | Level label's left edge relative to the canvas's left edge (in pixels). | 330 |
| levelY | Level label's top edge relative to the canvas's top edge (in pixels). | 130 |
| linesX | Lines label's left edge relative to the canvas's left edge (in pixels). | 330 |
| linesY | Lines label's top edge relative to the canvas's top edge (in pixels). | 160 |
| nextX | 'Next' label's left edge relative to the canvas's left edge (in pixels). | 330 |
| nextY | 'Next' label's top edge relative to the canvas's top edge (in pixels). | 260 |
| nextOffsetX | Next piece's left edge relative to the canvas's left edge (in pixels). | 330 |
| nextOffsetY | Next piece's top edge relative to the canvas's top edge (in pixels). | 280 |
| pauseX | 'Pause' label's left edge relative to the canvas's left edge (in pixels). | 145 |
| pauseY | 'Pause' label's top edge relative to the canvas's top edge (in pixels). | 220 |

### Colors

Color for the pieces is specified with a pair of values that determine the fill and border colors for the piece's individual squares (see [HTML canvas  fillStyle  Property](https://www.w3schools.com/tags/canvas_fillstyle.asp) and [HTML canvas  strokeStyle  Property](https://www.w3schools.com/tags/canvas_strokestyle.asp) for permitted for fill and border color values, respectively). Background and grid colors can also be changed.

| Parameter name | Description | Default value |
| --- | --- | --- | 
| zColor | Fill and border color for the Z piece's squares. | [ '#fe103c', '#f890a7' ] |
| sColor | Fill and border color for the S piece's squares. | [ '#66fd00', '#c4fe93' ] |
| oColor | Fill and border color for the O piece's squares. | [ '#ffde00', '#fff88a' ] |
| lColor | Fill and border color for the L piece's squares. | [ '#ff7308', '#ffca9b' ] |
| jColor | Fill and border color for the J piece's squares. | [ '#1801ff', '#5a95ff' ] |
| tColor | Fill and border color for the T piece's squares. | [ '#b802fd', '#f591fe' ] |
| iColor | Fill and border color for the I piece's squares. | [ '#00e6fe', '#86fefe' ] |
| gameOverColor | Fill and border color for the squares that flood the board on game over. | [ '#fff', '#ddd' ] |
| ghostColor | Fill and border color for the ghost piece's squares. | [ '#000', '#fff' ] |
| backgroundColor | Game board's background color. | '#222' |
| tetrisBackgroundColor | Game board's background color when burning a tetris.<br/>This color alternates with the default background color to create a flashing effect. | '#fff' |
| borderColor | Game board's border color. | '#fff' |
| gridColor | Game board's internal grid color. | '#ddd' |


### Sound

Although no music or sound effects are provided by default, it is possible to set these by providing [HTMLAudioElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLAudioElement) objects:

```javascript
const tetris = new ClassicTetris(canvas, {
  gameTheme: new Audio('korobeiniki.mp3'),
  // ... additional sound effects
});
```

| Parameter name | Description | Default value |
| --- | --- | --- | 
| rotateSound | Piece rotation sound. | undefined |
| moveSound | Left or right piece move sound. | undefined |
| setSound | Played when a piece locks on. | undefined |
| gameOverSound | Played on game over | undefined |
| lineSound | Sound of a single, double or triple line burn. | undefined |
| tetrisSound | Sound of a 4-line burn | undefined |
| levelChangeSound | Played when the level increases. | undefined |
| pauseSound | Played when pausing the game. | undefined |
| gameTheme | Theme song that plays throughout the game.<br/>Playback automatically pauses when the game pauses, and resumes when the game resumes. | undefined |

### Pointer parameters

The purpose of these is to help recognize the click / tap gesture when using a pointer device to control the game.

| Parameter name | Description | Default value |
| --- | --- | --- | 
| tapClickMaxDistance | Maximum distance between pointer-down and pointer-up coordinates (in pixels) for the game to count it as a click / tap. | 10 |
| tapClickMaxDuration | Maximum duration of a click / tap (in ms) to be regarded as such. | 500 |


## Functions

`ClassicTetris` provides the following functions to control the flow of the game.

| Function name | Description | Parameters |
| --- | --- | --- | 
| setStartLevel | Set the starting level for the next game.<br/>Has no effect if invoked while a game is being played. | level: an integer between 0 and 19 | 
| togglePlayPause | Starts a game if not playing, otherwise pauses/resumes the ongoing game.<br/>It is recommended to bind this method to the 'Play/Pause' button's `click` event, as shown in the ['Basic setup' example](#basic-setup). | none | 
| quit | If playing, terminates the game. | none | 
| on | Adds an event handler. | event: name of the event (see table below),<br/>handler: callback function to be invoked when the event fires. | 
| off | Removes an event handler. | event: name of the event (see table below),<br/>handler: callback function to be removed. | 

## Events

`ClassicTetris` dispatches events to notify about in-game occurrences.  Event listeners will be passed a single `data` parameter with details about the event.

| Event name | Description | Event `data` object properties |
| --- | --- | --- | 
| ClassicTetris.GAME_START | Fired at the beginning of a game. | type, level, score, lines | 
| ClassicTetris.GAME_OVER | Fired at the end of a game. | type, level, score, lines | 
| ClassicTetris.GAME_OVER_START | Signals the beginning of the game-over animation. | type, level, score, lines | 
| ClassicTetris.GAME_OVER_END | Signals the end of the game-over animation. | type, level, score, lines | 
| ClassicTetris.GAME_PAUSE | Fired when the game pauses. | type, level, score, lines | 
| ClassicTetris.GAME_RESUME | Fired when the game resumes after a pause. | type, level, score, lines | 
| ClassicTetris.PIECE_MOVE_LEFT | Fired when the current piece moves left. | type, piece, rotation, oldPosition, newPosition | 
| ClassicTetris.PIECE_MOVE_RIGHT | Fired when the current piece moves right. | type, piece, rotation, oldPosition, newPosition | 
| ClassicTetris.PIECE_MOVE_DOWN | Fired when the current piece moves down. | type, piece, rotation, oldPosition, newPosition, downPressed | 
| ClassicTetris.PIECE_HARD_DROP | Fired when the current piece is hard-dropped. | type, piece, rotation, oldPosition, newPosition | 
| ClassicTetris.PIECE_ROTATE_CLOCKWISE | Fired when the current piece rotates clockwise. | type, piece, position, oldRotation, newRotation | 
| ClassicTetris.PIECE_ROTATE_ANTICLOCKWISE | Fired when the current piece rotates anticlockwise. | type, piece, position, oldRotation, newRotation | 
| ClassicTetris.PIECE_LOCK | Fired when the current piece locks on. | type, piece, rotation, position | 
| ClassicTetris.NEXT_PIECE | Fired whenever a piece is generated. | type, piece, nextPiece | 
| ClassicTetris.LEVEL_CHANGE | Fired when the level changes. | type, oldLevel, newLevel | 
| ClassicTetris.SCORE_CHANGE | Fired when the score changes. | type, oldScore, newScore | 
| ClassicTetris.LINE_CLEAR_START | Signals the start of the line-clear animation. | type, linesBurnt | 
| ClassicTetris.LINE_CLEAR_END | Signals the end of the line-clear animation. | type, linesBurnt | 
| ClassicTetris.LINE_CLEAR | Fired when one or more lines are cleared. | type, oldLines, newLines | 



## License

`classic-tetris-js` is released under the MIT License. See [LICENSE](LICENSE) for details.