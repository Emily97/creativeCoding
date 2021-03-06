# Color > Color Palettes From Rules Part 01
## Step One

- [ Step One ](Step 01/)

```js
'use strict';
// declaration of global variables that determine the number of tiles in the x and y direction
var numberOfColumns = 50;
var numberOfRows = 10;

function setup() {
  //the canvas is created to fit the browser window
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100);
}

function draw() {
  //tiles width and height are calculated by dividing into numberOfColumns and numberOfRows respectively
  var tileWidth = width / numberOfColumns;
  var tileHeight = height / numberOfRows;

  //using a loop to create rows and columns of tiles all preset to color red
  for (var gridY = 0; gridY < numberOfRows; gridY++) {
    for (var gridX = 0; gridX < numberOfColumns; gridX++) {
      var posX = tileWidth * gridX;
      var posY = tileHeight * gridY;
      fill('red');
      rect(posX, posY, tileWidth, tileHeight);
    }
  }
}
```

## Step Two

- [ Step Two ](Step 02/)

```js
'use strict';

var numberOfColumns = 50;
var numberOfRows = 10;

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100);
}

function draw() {
  // limit mouse coordinates to the canvas
  var mX = constrain(mouseX, 0, width);
  var mY = constrain(mouseY, 0, height);

  // mapping the range of values for number of columns and rows on screen
  // in this case the max number of columns generated is 50
  var mouseNumberOfColumns = int(map(mX, 0, width, 1, numberOfColumns));
  var mouseNumberOfRows = int(map(mY, 0, height, 1, numberOfRows));
  var tileWidth = width / mouseNumberOfColumns;
  var tileHeight = height / mouseNumberOfRows;

  for (var gridY = 0; gridY < numberOfRows; gridY++) {
    for (var gridX = 0; gridX < numberOfColumns; gridX++) {
      var posX = tileWidth * gridX;
      var posY = tileHeight * gridY;
      fill('red');
      rect(posX, posY, tileWidth, tileHeight);
    }
  }
}
```

## Step Three

- [ Step Three ](Step 03/)

```js
'use strict';

var numberOfColumns = 50;
var numberOfRows = 10;
//declaring global variables that are empty arrays that will be filled with values of Hue, Saturation and Brightness.
var hueValues = [];
var saturationValues = [];
var brightnessValues = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100);
  noStroke();

  // initializing random values for each column tile which will later generate different color tiles.
  for (var i = 0; i < numberOfColumns; i++) {
    hueValues[i] = random(360);
    saturationValues[i] = random(100);
    brightnessValues[i] = random(100);
  }
}

function draw() {
  var mX = constrain(mouseX, 0, width);
  var mY = constrain(mouseY, 0, height);

  var counter = 0;

  var mouseNumberOfColumns = int(map(mX, 0, width, 1, numberOfColumns));
  var mouseNumberOfRows = int(map(mY, 0, height, 1, numberOfRows));
  var tileWidth = width / mouseNumberOfColumns;
  var tileHeight = height / mouseNumberOfRows;

  for (var gridY = 0; gridY < numberOfRows; gridY++) {
    for (var gridX = 0; gridX < numberOfColumns; gridX++) {
      var posX = tileWidth * gridX;
      var posY = tileHeight * gridY;
      // counter gets incremented with each loop
      // when the counter exceeds the maximum number of columns the counter is reset to 1
      var index = counter % mouseNumberOfColumns;

      // gets the values stored in the arrays at the same index value to create the tile colors
      fill(hueValues[index], saturationValues[index], brightnessValues[index]);
      rect(posX, posY, tileWidth, tileHeight);
      counter++;
    }
  }
}
```

## Final Step

- [ Final Step ](Final/)

```js
'use strict';

var numberOfColumns = 50;
var numberOfRows = 10;

var hueValues = [];
var saturationValues = [];
var brightnessValues = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100);
  noStroke();

  for (var i = 0; i < numberOfColumns; i++) {
    hueValues[i] = random(360);
    saturationValues[i] = random(100);
    brightnessValues[i] = random(100);
  }
}

function draw() {
  var mX = constrain(mouseX, 0, width);
  var mY = constrain(mouseY, 0, height);

  var counter = 0;

  var mouseNumberOfColumns = int(map(mX, 0, width, 1, numberOfColumns));
  var mouseNumberOfRows = int(map(mY, 0, height, 1, numberOfRows));
  var tileWidth = width / mouseNumberOfColumns;
  var tileHeight = height / mouseNumberOfRows;

  for (var gridY = 0; gridY < numberOfRows; gridY++) {
    for (var gridX = 0; gridX < numberOfColumns; gridX++) {
      var posX = tileWidth * gridX;
      var posY = tileHeight * gridY;
      var index = counter % mouseNumberOfColumns;

      fill(hueValues[index], saturationValues[index], brightnessValues[index]);
      rect(posX, posY, tileWidth, tileHeight);
      counter++;
    }
  }
}
// when a key is pressed this function is called
// when s is pressed the canvas is saved as a png with the file name being the date and time it was captured
// when the c code is pressed the color values of the tiles are saved as an ase file which can be used as a palette in photoshop
function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
  if (key == 'c' || key == 'C') {
    var colors = [];
    for (var i = 0; i < hueValues.length; i++) {
      colors.push(color(hueValues[i], saturationValues[i], brightnessValues[i]));
    }
    writeFile([gd.ase.encode(colors)], gd.timestamp(), 'ase');
  }
  //these key presses change the HSB values depending on what is specified for each value under each key
  // red to orange hues
  if (key == '1') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(1,50);
      saturationValues[i] = random(100);
      brightnessValues[i] = random(100);
    }
  }
  //bright yellow to green
  if (key == '2') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(51, 100);
      saturationValues[i] = random(100);
      brightnessValues[i] = 100;
    }
  }
  //saturated green
  if (key == '3') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(101, 150);
      saturationValues[i] = 100;
      brightnessValues[i] = random(100);
    }
  }
  //greens to blues
  if (key == '4') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(151, 200);
      saturationValues[i] = random(100);
      brightnessValues[i] = random(100);
    }
  }
  //saturated blue
  if (key == '5') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(201, 250);
      saturationValues[i] = 100;
      brightnessValues[i] = random(100);
    }
  }
  //bright indigo to violet
  if (key == '6') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(251, 300);
      saturationValues[i] = random(100);
      brightnessValues[i] = 100;
    }
  }
  //violet to red
  if (key == '7') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = random(301, 360);
      saturationValues[i] = random(100);
      brightnessValues[i] = random(100);
    }
  }
  //black and white
  if (key == '8') {
    for (var i = 0; i < numberOfColumns; i++) {
      hueValues[i] = 0;
      saturationValues[i] = 0;
      brightnessValues[i] = random(100);
    }
  }
  //random saturated colors
  if (key == '9') {
    for (var i = 0; i < numberOfColumns; i++) {
      if (i % 2 == 0) {
        hueValues[i] = random(360);
        saturationValues[i] = 100;
        brightnessValues[i] = random(100);
      } else {
        hueValues[i] = 195;
        saturationValues[i] = random(100);
        brightnessValues[i] = 100;
      }
    }
  }
  //green and indigo
  if (key == '0') {
    for (var i = 0; i < numberOfColumns; i++) {
      if (i % 2 == 0) {
        hueValues[i] = 140;
        saturationValues[i] = random(30, 100);
        brightnessValues[i] = random(40, 100);
      } else {
        hueValues[i] = 270;
        saturationValues[i] = random(40, 100);
        brightnessValues[i] = random(50, 100);
      }
    }
  }

}
```
