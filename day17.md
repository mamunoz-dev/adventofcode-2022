# Day 17: Pyroclastic Flow

Codepen: https://codepen.io/mamunoz-dev/pen/JjBOmjw?editors=0012

```js
class PyroclasticFlow {
  shapes = {
    minus: [[0, 0], [0, 1], [0, 2], [0, 3]],
    plus: [[0, 1], [1, 0], [1, 1], [1, 2], [2, 1]],
    angle: [[0, 2], [1, 2], [2, 0], [2, 1], [2, 2]],
    minusVertical: [[0, 0], [1, 0], [2, 0], [3, 0]],
    square: [[0, 0], [0, 1], [1, 0], [1, 1]],
  };
  shapeKeys = Object.keys(this.shapes);
  chamberWidth = 7;
  shapeIndex = 0;
  jetIndex = 0;
  chamber = [];

  rockHeight = 0;
  fallenRocks = BigInt(0);
  repeatingHeight = BigInt(0);

  cache = {};
  cacheSize = 30; // top rows of chamber

  constructor(input, totalRocks) {
    this.input = input;
    this.totalRocks = BigInt(totalRocks);
  }

  getNextShape() {
    if (this.shapeIndex >= this.shapeKeys.length) this.shapeIndex = 0;
    return this.shapes[this.shapeKeys[this.shapeIndex++]];
  }

  getNextJet(){
    if (this.jetIndex >= this.input.length) this.jetIndex = 0;
    return this.input[this.jetIndex++];
  }

  addRows(rows) {
    for (let i=0; i<rows; i++) {
      this.chamber.unshift(new Array(this.chamberWidth).fill("."));
    }
  }

  getShapeHeight(shape) {
    return Math.max(...(shape.map(([y]) => y))) + 1;
  }

  moveShape(yOffset, xOffset, shape) {
    return shape.map(([y, x]) => [y + yOffset, x + xOffset]);
  }

  fallOneUnit(shape) {
    return this.moveShape(1, 0, shape);
  }

  applyJetPush(shape) {
    const jet = this.getNextJet()
    return jet === "<" ? this.moveShape(0, -1, shape) : this.moveShape(0, 1, shape);
  }

  isCollision(shape) {
    return shape.reduce((acc, [y, x]) => {
      return acc || x < 0 || x > this.chamberWidth - 1 || y >= this.chamber.length || this.chamber[y][x] !== ".";
    }, false);
  }

  addRocks(shape) {
    shape.forEach(([y, x]) => {
      this.chamber[y][x] = "#";
    })
  }

  cacheRepeatingPatterns(minY) {
    // Let's create some cache entries for this to see if we find a repeating pattern
    const fragment = this.chamber.slice(minY, minY + this.cacheSize).flat();
    const cacheKey = this.shapeIndex + ":" + this.jetIndex + ":" + fragment.join("");

    if (cacheKey in this.cache) {
      const [cFallenRocks, cRockHeight] = this.cache[cacheKey];

      // Amount of rocks / height after pattern repeats
      const diffFallenRocks = BigInt(this.fallenRocks) - BigInt(cFallenRocks);
      const diffRockHeight = BigInt(this.rockHeight) - BigInt(cRockHeight);

      // Calculate the leap that can be made towards total rocks
      const remainingRocks = BigInt(this.totalRocks) - BigInt(this.fallenRocks);
      const repeat = BigInt(remainingRocks) / BigInt(diffFallenRocks);

      // Leap forward by adding the repeating rocks and height
      this.fallenRocks += repeat * diffFallenRocks;
      this.repeatingHeight += repeat * diffRockHeight;
    }

    // Only cache large repeating blocks
    if (cacheKey.length > this.cacheSize * this.chamberWidth) {
      this.cache[cacheKey] = [this.fallenRocks, this.rockHeight];
    }
  }

  getTowerHeight(cacheActive=true) {
    // Initial chamber
    this.addRows(10);

    while (this.fallenRocks < this.totalRocks) {
      let shape = this.getNextShape();
      const shapeHeight = this.getShapeHeight(shape);

      this.addRows(shapeHeight);

      const syOff = this.chamber.length - this.rockHeight - 3;
      // left edge is two units away from the left wall
      shape = this.moveShape(syOff - shapeHeight, 2, shape);

      let groundCollision = false;
      while (!groundCollision) {
        let movedShape = this.applyJetPush(shape);

        if (!this.isCollision(movedShape)) {
          shape = movedShape;
        }

        movedShape = this.fallOneUnit(shape);

        if (this.isCollision(movedShape)) {
          this.addRocks(shape);

          this.fallenRocks += 1n;
          const minY = Math.min(...(shape.map(([y]) => y)));
          this.rockHeight = Math.max(...([this.rockHeight, this.chamber.length - minY]));
          groundCollision = true;

          if (cacheActive) {
            this.cacheRepeatingPatterns(minY);
          }

          break;
        }

        shape = movedShape;
      }
    }

    const answer = BigInt(this.rockHeight) + BigInt(this.repeatingHeight);
    return answer;
  }
}

const sample = '>>><<><>><<<>><>>><<<>>><<<><<<>><>><<>>';
const input = '>>>><>><<>>>><<<>><<<<>><<<>>>><<<>>><<<<>><<>>>><<>>>><<<<>>>><><<<<>>><<>>>><<>>>><<><<<<>>>><<>>>><><<<>>>><<>><<<>><<<<>>>><<<<><>>>><<<><>>><<<<>>>><<>><<>><>><<<><<<<>>>><<<><><<>>><<>>><>>><>><>>>><<<<>>><>><<><>><<<>>>><<<<><><<>><<<>>><><<<<>>>><<<>><<<>>><<<>>><<<><<<>>>><<<<>>><<<<>><<><<<>>>><<<>>>><<<<>><><>><<>><<><<>>><>>><>>><<<><<<<>><<<><<<>><<>><><<<>><<<<><<<>>><<>>><>>><<<><>>>><<>>>><<>>><<<>>><<<<>><><<<>><>>>><<><><<>>>><<<<>>><<><<><<><<<>>>><<<>><<<><<>><>><><<<<>><<<>><<>>>><><<<<><<<<>>><<>>>><<<<><<<>>><<>>>><<>><<<>>><<>>><<>><<<>><<<>>><<>>><<>>>><><<>><<<>><<<><><<<>>>><<<>>>><<<>><<<<><<>>><<<>>>><<>>><>>>><<<>>>><<><>><<<>><<<<>>>><>>><<<><<>>><<<>>>><<<<>>>><<>>>><<><<><<<<><><<<>>><<>>><<<<>><<>>><>>>><<<<>><<>><<<<>>><<>>>><<<<>>>><<<<>><<<><<<<>><<<<>><<>><<>><<<<>>><<<>>><<<<>><>><<<><<>>>><<<<>>>><<<<>>><<<<>><<>>>><><><>>><<>>><<<>>>><>><<<<><<<<><><<>>><<<>>>><<<<>>><>><<<<>>>><>><<<>>><>>>><<<>>>><<>>><<<><<>>><<><<<<>>><<<<>>>><<>><>>><<>>><>>><<<>>><<<<>>><<<><>>>><<<<>><>>><<<>><<<>>><><<<><<><>><<<<>>>><<<>>>><<<><<<>><<<<>>>><<<>>>><<>><<<>><<<>>><<><<<<>>><<<<>><>>>><<>>>><<<>><>>>><<<>>><<>>><><>>>><<<>>>><>>>><<<>>><<<>>>><<>>>><<><<<>><<><>>>><<>>><<>>><>>><<<>>><<><<<><<>><<>>><<<<><<<><<<<>>><<<<><>><<>><<<>>><>><<><><<<<>>>><<>>>><<<<>><<<<>><>>><<<><<<<>><<<<>>><<<><<<>>>><>>><>>>><<<<>><<<<><><>>>><<<>>><<<><<<<><>><<<>>>><<<>>><<<<>>><<<<>><<<>><<<>><<<<>>><>>>><<<><<<>>><>><<>>>><<<>>><>>><<<<>>><<<>>>><>>><<><<>>><<>>>><<<><<<>><<>><<>>>><<<>><>>>><<<>>>><<>><>><>>>><<<>>><<>>><>>><<<>>>><<<>><>>><<<>>>><<<<>>>><<<><<><<>><<<>>>><<<<>>><>>><<>>><<<>><<>>><>>>><<<<>><<<<>>>><<>>>><><<<<>><>><<><><>><<<>><<<<>>><<<>>><<<<><<<<>>>><<<<>>>><<<<>>>><>><<>>>><<<<>><>>><<<<><<>>>><<<<>>><><<<<>>>><><<>>><<<<>>>><<<>><<><<><<>>><><>>><<<<><<<>>><<<<><<<<>>>><<>><<>>>><<<<><<<>>>><>><<<<>>><>><<<><>>><<<><>>><<><<>>><<<<><<<<><<>>>><>>><>>><>><>>>><<<<>>><<<<>>><<>>>><<<>>><<<<>>>><><>>><<<<>><<<><<<>>>><<<<>>><<<<>>><<<>>><<<<>><>><<>><<>>><<<>><>>><<>><<<<>>>><<>><<<>><<>><<<>>><<<>>>><<<>>><<<>>>><<>><<><>>><<<>>>><<<<>>>><<<>>>><>>>><<<>>><<<<>>>><<<>>><>>><>>><><>>>><<<>>>><><>>><<><>><<<<>>><<<<><<<<>>>><<<<>>><>><<<>>><<<>><<<<>>><<<<><<>><<><<><<<<>>>><<>><>><>>><>><<<>>>><<<>>>><<>><<><<<<>>><<><<<>><<<>>><<><<<<>>><<>><>><>>>><<><<<>><<<><<<<>><>><<<><>><<<>><<<>>>><<<<>>><<<<>>><<<><<>>>><<<>><<<<>><<<>>><<<<>>><<<<>><<>><<<>>>><<><<<>><<>>><<<<>>>><<<>><<<<>>>><<>>><<<<><<<>>><<<>>>><>>><<<<><<<>><<<><<<>><><<>>>><>>>><<<<>>>><><<<<><<>>>><><<<><<<>>><<>>>><<<<><><<<<>>><<<<>>><<>>><<>>><<>>><<>>><<<<>><>>><<>><<<>>><><>><<<>>><><>><<>><<<>>>><<<<>>>><>><<<<>>><><<<<>><<>>>><>><<>>>><>>><<<<>><<<<>>><>>>><<<>><>>><<<<>><<<<>><<>>><>>>><<<>>><<<<>>><<<>><<>>><<<>><<<<>>><<>>><>>>><<<>>>><<<>>><<<<><<>><<<<>>>><><<<<>>><><<>>><<<<><<>>>><<<<>>><<<<>>><<<<>><<<<>><<>><<<<>>>><<<>><<<><<<>>>><<>><<>>><<<<><<<<>>><<<><<<<>>>><<>><<>><<<>>><<<<>><<<<>><<<>>><>><<<><<<>><>>>><<<>><<<<>>><<>><<<>><>><>><<<>>><<<>>><<<><>><><>>>><<<>>><>>><<>>>><<<>>>><<><<><>>><<<>>>><<<>><<<>>>><>>>><<<><<>><><>><<<>>>><<<<><<<>>>><<>>>><<<<>>>><<>><>>><<<<>><<<<><>>>><<<>><<>>>><<<<><<<>>><<<>>><<<>>>><<<<>><>><>>><<>>><>>><<<<>><<<<><>>><<<>>><<<><<>><>><>>><<<>>><<<<>>>><<<<>>>><<>><<<>>>><>>>><<<>><>>>><<<<>><<<<>>><>><<<<>><<><<<>>>><<<>>><>><<>>>><>>>><<><><<>>><>><<<<>>><<<<><>>><<<>>>><<>>><<<><<<<>>><<<<>>><><<>>><<>><><<>>><>>><><<<>>><>>><<<<>>><<<>><<<><<<<><<>>>><<<<>>><<<>>>><>>><<>><>>><<<>>><><<<>>>><<<>>>><<<>>><<>>><<<>>>><<<>>><<>><><>>><<>>><<<<>>><<<><<<>>><<<<>><<<>>><<><<><>>>><<>><>>><<>>>><<<<>><<<><<<>>>><>>>><>><<<<>>>><<>>>><<<<>>>><<<>>>><>>>><>>><<<>>>><<<>>>><<>>>><<<<>>>><<>>>><<<><<<>>><<<>><>><<<>><<>><>>><<<>>>><<<>>>><<><<<>>>><<<><><<<<>><>>><<><<<<><<>><<<<>>>><<>><<<>>>><<><>><>>><<<>><>><<<<><>>><>><>><<<<>><>>>><<>>>><<>>><>>><<<<>><>>>><<<<>>>><<>>>><<>><<<<>><>>>><>><>><>>>><><<>><<<>>><<<>>>><<<><<<><<<<>><<<<>>>><<<>><<<>>><<<>>><>>>><>>><<<<>>><<<<>>><<<<>>><<<>><<<>><<<<>>>><<<>>>><<<>>>><<<>>><<>>><<<<>>>><<><<<>>><<><>><><<>>>><<>>><<>>><><<<>><<><<<>>>><<>><<<<>>><>>>><>><<><<<<>>>><>><<>><<><<<>>><<>>><<<<><>><<<>><><<<<><<>><<<<>>><<><<<>>>><<<<><>><<<><<<<>><<<<>>>><>>><<<<>>>><<>>><<<>>>><<<<>><<<>>>><<<>>>><<<<>>><<<<><<<><<>><<<<>>>><<<<>>>><<>>><<<>>>><<<>>>><<>>><<>>><<>>><<>>>><<>>><<>>><<><<<<>>>><>>>><<<><<>>><<>><<<<>><>>>><><<>>>><<<<>>><<>><<<>><<<>>>><<><><<>><<<<><>><<<<>>><><<>>><>><<<>>><<<<><>><>><<>>>><><<<>>>><<<<><<<>><<>>><<>><<<<>><<>>><>>>><<<<>><>>><>>><<>>>><<<>>>><<>><<>>>><>>>><<<<>>>><>>><<<<>>>><<<>>><<><<<>><<<>><<><><<<>>>><<<<>><<<><<<<>>><<<<><<<><<>><<>><<>>><>>>><<<<>><<<>>><>>>><<>><<<<>><>><>>><<<<><<<><<<<><<<<>>>><<<<>>>><>>>><<<>><<>>><><><>><<<>>><<>><<<<>>><<<>><<<>><<><<<<>><>>>><<<>>><<<><>>><>>><<>>><><<<<>><<<>><<>><<>>>><<<>>><><<<>>>><>>>><<<<>>>><<<<><<>>><<>>>><>><<<>>>><<>><<>><>>><>>><<>>><<<<>><>>>><>>><>><<<<><<>>><<<>>><<<>>>><<<>><<><<>><<>>>><<>>><>><<>>><>>>><<<<>>><<>><>><<<><>>>><<>>>><<<>>>><<>><<>>>><<>><<<>><<<<>>>><><<>>>><>><<>><<<>>><<<<>>>><<>>>><<><<>>>><<>><<>>>><>>><<<<>>>><>>><<<>>>><<<>><<<<>>>><<<>>>><>>><<<>>><>><>><<<>><<<><<>><<<>><>>>><>>><>>><<>>><<<>>>><<<<><<<>><<<<>><<<<>><>>>><<<>><<><><><<<<><<<<>>>><<>>><>>>><><<<<>>><>><>><>>><<<>>>><>>>><<><<<<>>><<<<>>><<>><><><><<<>>><<<<>><<>><<<>>>><>>><>>><<>>>><<<<>><<>>><<>><<<><>><<<<>><<>>><<<><<<>>>><<<><<><>>>><<>>>><<<<>><<>><<<><<<>>><>><<<>><<<<><<<>>>><>>>><<<>>>><><>>>><<>><<<<>>><<>><<>><>>><<<<>>><<>><<<>>><<<<><<<<>>><<<><<>>><<<<><<>>>><>><<<<>><><<<<>>><<<>>>><<<<>><><<>><<<>><>>>><<<>>>><<<>>><>><<><<<>><>><<>>><><<<<>>>><<<<>>>><<>>>><>>><<>>><<<><>>><<>>>><<<<>>>><<<>>>><>><<<<>>><<>><<<<>><<<<>><<><<<><>>>><<><<<>>>><>><<>>><>>><<>>><<<>>>><<<<>>>><<>><<<<><<<<><><<<>>><<<<>><<>>><<<>>><<<<>>><<<>><<><<<<>><<><<<>><<<<>><<<<>>>><>>><<<<>><<>><>>>><<>>>><<<<>>>><<<>>>><<<<><<<<>><>><>><<<<>><>><<<<><<<>>>><<<<><<>>>><>><<<<>>><<<>>><><>>><<<>><>><<>><>>>><>><<<>>><<<>><<<<>>><<>>>><<<<><<<>>>><<<<><<>><><<<>><>><<<<>>>><<<>>>><<<>>>><><<<<><<<<>><<<><<>><<>>><<<>>><>>>><<<>>>><<<>><><<<<><<<><><>>>><<<>><<<>>>><<<>>><<<>><<><<><>><<><<<<>>><<<<><><><<>>>><>><<<<>>>><<<<>>><<><<>><<>>><>>>><<<>><<<<><<<><<<<>><<<>>><<>>><<<<>><>>>><<<<>><>><><<<<>>><<<>>><<>>>><<<<>>><<<><<>><<<<><<<><<<<>><<><<><<>>><<<>>>><><<<>>>><<<<>>>><<<>><<<<>><<<<>>><>><<<<>>><>>><<<>><>>><<>>>><>>>><<>><>>>><>><>>><<<>>><>>><<>><<<<>>>><<<<>>><<>>>><<<<>>>><<>>><<>>>><<<>>><<<>>><>>><<<>>><<<>><><<<><<<<>><<>>><<>>><>>><<>><<>>>><<>>><<<>>>><<>>><<<<>><<>>>><>>><<<>>>><<>><>><>>>><<>>><>><><<<<>><<>>><<<<><>>>><<<>><><>>>><<<<>>>><<<>>>><<<>>>><>>><<<><><<<<><>>>><<>>>><<<<>>>><<<<><<<<>><<<<>>>><<<>>>><<><<>><<<>>>><<><<<<>><>>>><<>>>><>>><<<<>>>><<>>>><<<><>>>><<<<>>>><<<<>>><<<<>><><<>><<<><<><<<>><<>><>>>><<><>>><<<>><<<>>><<><<><<<<><>><<><<<>><<<<>><>>><>>>><<<>>>><>>>><<<>><>>>><><>>><<<<>>><<<<>>>><<><><<<><<<>>>><<<>>><<<>><<>>>><<><<<>><<<>><<>>>><<>><<<>>><<<<>>><<<<>><<><>>>><<<>>><<><>><<>><>>>><>>>><>>><<<<>><<><>>><<<<><<<>>>><><<>>>><<<>>>><<>>><<<<>>>><<<<>><>>>><>>>><<<<>>>><<<<>>><<<<><<>><<>>><<<>><<<<><<<>><<>><<<>><<<>><<<>>><<<>>><<>>>><>>>><<<<>>><<<<><<>>>><<>><<<<>><>><<>><<<><<<>>>><>><<<<><<<<>><>>><<>><<<>>>><<<>>>><<>>><<<>>><<><>>>><<<>>><><<<<>><<<><<<<><>>>><>>><<>><<><><><<><>><<<>>>><>>>><<>><<<>><<<<>>>><<>><<<><<>>>><<<<>>><<>>>><<<<>>>><<<>><<><><<<<>>><<>>>><<>>>><<<<>>>><<<<><<<>>>><<<<>><<<<>>><<<<><<<<><<><<<<>>><<>>>><>>><><<<<>><<>>><<<<>>>><<<>>><<<<><><<<<><<>>>><<>><<<<>><><<<<>>><<<<>>>><<>>>><<<<>><<<<><>>><<>>>><<<>>><<><>>><<<>>>><<<><<<<><<><<<>><<<<><<<>><><<<>><><>>><<<<>>><<<>>>><><<<>><<<<><<<<>><<<>><<>><><>>><<<>><<<<>>><<<><>>><<<><>><<>><<>><<<>>>><<<>>>><><<><<<<>>><<>>>><<>>><<>><<<>><<<<>>>><<<><<<><<><<<>>><><<<>>>><<><<>><<<>><>><<>>>><<<>>><<><<>><>><<<<>>>><<>>><<><<<<>>>><<><<>>><>>><<>>>><<<<>><<>><<<<>>>><<<>>><>><><<<>><<<<>><>><<<>><<>><<>><<<<>><<<<><><<<>>>><<<<><<<<>><<<<>>>><<>>><<<><<>>><<<<><<<<>>>><<<>>>><<<<><<<>>><<<>>>><<<<>><<<>>><<<<>>><<<>>>><>>>><<<>>><<<>>>><<<<>><<<<>>><<>>>><>>>><<>>>><<<>><<<<>>><<<>>><>>><<<>>>><<>>>><>>><<><>><<>>>><<><<<>>><<<<>>><<>>><<<>><<<>>><<>>><<<>>>><<<>>>><>>><<<<><>>>><<<>>><><<<><<<>><<<>>><<<<>><<>><<<<>>><<<>>><><<>>><<<>>>><>><<>>>><<<>><<<<>><><>>><>>><<>>>><<>>><><<<<>>>><<>>>><<<<>>>><<<<>><<<<>>>><<>>>><<>>><>><>>><<<>>>><<<><<>>><<<<><<<><<>>>><>><<<<>><<>>><><<<><<<><<<>>>><>><><<<<>>>><<<<>>><>>><<<>>>><>><<<>><<><<<<>><>>><>>>><<<<>><<>><>><<<>>>><><>>>><<>>>><>>><<<><<<>><<><<>>><<<<>><<>>><<>>><<<<>>><<<>><<<>><<>><<<<>>>><><<><<<<><<>><<<<><<>>><<>>><<><<<>>><<><<<<>>>><<<>>><<<<>>><<<>>><>>><<>><<<><<<>>>><>><<<<>><<<>>>><<<><<>><<<<>><>><<<<>><<<<>>><<<<>>><>><<<>>><>>><>>>><>>>><<<>>><<<><<<>><<>>>><<<>>><><<>>><>>><>>><<>>>><>>>><<<>><<<><>><<<<>>>><<>>><<<<>>><<<>><<<>>><<<>><>>>><<<<><<<<>>><>>><<<<>>>><<<>><<<<><<>>><<>>>><<<><<<><>>>><<>>><<<<><<<<><<<<><<<<>>><<<<>>><<>><<<><<<>>><<>><>>>><>>><<<<>><<>>><<>>><<<<><<<>>>><<><>>><<<<><<<><<<<>>>><>>>><<<<>>>><<>>>><<>>><<<>>>><<>>><<>><<<<>><<>>>><<><<<<>>>><<<<>>><<><>><<<>><>><><>>>><<<>>>><<<<>>>><<>><<>>><<<<><<<>>><<<>>>><>>>><<>>><<>>><<<>>>><<>>><>>>><<>>>><<<<>>><>><<<>>><<<<>>><>>>><>>>><<<<>>><>>><>><<>>><>><<<<>>><>>>><>>><<<><<><<<<><<<<>>>><<>>><<<<><<<>>>><<<<>>>><<>><<<>>>><<<<>><<>>>><<<>>><>><<<<>>>><<<<>>>><<<<>><><<<<>>><<<<>><<<<>>><<<>><<>><<>>>><<<<>>>><<><>>><<<>>>><<<<>>>><<<>>><>>>><<>>><><<<<>><<<><<>>><<<<><<>>><<<><<<><<<<>>>><<>><>>><<<<>>>><>><><<<<>>><<>>><<<>>><>>>><>>>><><>>><<<<>>><<<>>><<><<<>><<<>>><<<>>><<<<>><<<>>>><<<>>>><<<>><<<<><<<<>><><>>><<>>><<<<><<><<>><<<<>>><<<<>>><<<<>><<><<<>><><<>><<<>>>><<<><<<><<<<>>>><<<><<<<>><<<>>>><>>><<<<>>><<<<>>>><<>><><<<>>>><<>><<<<>>><<<>><<<<><<<>><>>><<<><<<<><<<>>><<<<>>><<<<><<>>><>><<<<>><<><<>>><<<<><<>>><<><<>>>><<<<><<<><>>><<>>>><<>>>><><<>><<<>>><>><<><<<<>>>><<>>>><<<>>>><>>><<<>><<<>>><><>><<>><>><<>><<<><<<<>>>><<<>>><<>><<<>>><<><>>><<>><<<>>><<<><<>>>><>><>>><><<<>>>><<<<><<<>><<<><<<><>>><<<>>><<>><>>>><>>><<';

const samplePart1 = new PyroclasticFlow(sample, 2022);
const inputPart1 = new PyroclasticFlow(input, 2022);
const samplePart2 = new PyroclasticFlow(sample, 1000000000000);
const inputPart2 = new PyroclasticFlow(input, 1000000000000);

console.log('part 1 sample: ', samplePart1.getTowerHeight(false)); // 3068
console.log('part 1 input: ', inputPart1.getTowerHeight(false)); // 3166
console.log('part 2 sample: ', samplePart2.getTowerHeight()); // 1514285714288
console.log('part 2 input: ', inputPart2.getTowerHeight()); // 1577207977186
```
