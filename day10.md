# Day 10: Cathode-Ray Tube

Codepen: https://codepen.io/mamunoz-dev/pen/QWBOaZG?editors=0012

```js
function getCyclesDistribution(input) {
  return input
    .map(opp => {
      const [cmd, x] = opp.split(' ');
      return cmd === 'noop' ? [cmd] : [cmd, opp];
    }).flat();
}

function getValueOfXAfterCycle(cycleNum, cycles) {
  return cycles
    .slice(0, cycleNum-1)
    .reduce((acc, opp) => {
      const currentX = opp.split(' ')[1] || 0;
      return acc + Number(currentX);
    }, 1);
}

function getSignalStrength(cycles) {
  const cycleNumbers = [20, 60, 100, 140, 180, 220];
  return cycleNumbers
    .reduce((acc, cycleNumber) => acc + cycleNumber * getValueOfXAfterCycle(cycleNumber, cycles), 0);
}

function getSpritePosition(x) {
  return [x-1, x, x+1];
}

function getCrtChar(currentCycle, cycles, col) {
  const x = getValueOfXAfterCycle(currentCycle+1, cycles);
  return getSpritePosition(x).includes(col) ? '#' : '.';
}

function getCrtRow(row, cycles) {
  const crtTotalCols = 40;
  const blockOf40Cycles = cycles.slice(crtTotalCols*row, crtTotalCols*row + crtTotalCols);
  const emptyCrtRow = '.'.repeat(crtTotalCols).split('');

  const crtRow = blockOf40Cycles.reduce(
    (crt, cycle, col) => {
      const currentCycle = crtTotalCols*row + col;
      crt[col] = getCrtChar(currentCycle, cycles, col);
      return crt;
    },
    emptyCrtRow
  );

  return crtRow.join('');
}

function getCrt(cycles) {
  const crtTotalCols = 6;

  const crt = Array.from(new Array(crtTotalCols))
    .map((row, rowIndex) => getCrtRow(rowIndex, cycles));

  return crt;
}

function printCrt(cycles) {
  const crt = getCrt(cycles);
  for(let i=0; i<crt.length; i++) {
    console.log(crt[i]);
  }
}

function part1(input) {
  const cycles = getCyclesDistribution(input);
  return getSignalStrength(cycles);
}

function part2(input) {
  const cycles = getCyclesDistribution(input);
  printCrt(cycles);
}

const sample = ["addx 15", "addx -11", "addx 6", "addx -3", "addx 5", "addx -1", "addx -8", "addx 13", "addx 4", "noop", "addx -1", "addx 5", "addx -1", "addx 5", "addx -1", "addx 5", "addx -1", "addx 5", "addx -1", "addx -35", "addx 1", "addx 24", "addx -19", "addx 1", "addx 16", "addx -11", "noop", "noop", "addx 21", "addx -15", "noop", "noop", "addx -3", "addx 9", "addx 1", "addx -3", "addx 8", "addx 1", "addx 5", "noop", "noop", "noop", "noop", "noop", "addx -36", "noop", "addx 1", "addx 7", "noop", "noop", "noop", "addx 2", "addx 6", "noop", "noop", "noop", "noop", "noop", "addx 1", "noop", "noop", "addx 7", "addx 1", "noop", "addx -13", "addx 13", "addx 7", "noop", "addx 1", "addx -33", "noop", "noop", "noop", "addx 2", "noop", "noop", "noop", "addx 8", "noop", "addx -1", "addx 2", "addx 1", "noop", "addx 17", "addx -9", "addx 1", "addx 1", "addx -3", "addx 11", "noop", "noop", "addx 1", "noop", "addx 1", "noop", "noop", "addx -13", "addx -19", "addx 1", "addx 3", "addx 26", "addx -30", "addx 12", "addx -1", "addx 3", "addx 1", "noop", "noop", "noop", "addx -9", "addx 18", "addx 1", "addx 2", "noop", "noop", "addx 9", "noop", "noop", "noop", "addx -1", "addx 2", "addx -37", "addx 1", "addx 3", "noop", "addx 15", "addx -21", "addx 22", "addx -6", "addx 1", "noop", "addx 2", "addx 1", "noop", "addx -10", "noop", "noop", "addx 20", "addx 1", "addx 2", "addx 2", "addx -6", "addx -11", "noop", "noop", "noop"];
const input = ["addx 1", "noop", "addx 5", "addx -1", "addx 5", "addx 1", "noop", "noop", "addx 2", "addx 5", "addx 2", "addx 1", "noop", "addx -21", "addx 26", "addx -6", "addx 8", "noop", "noop", "addx 7", "noop", "noop", "noop", "addx -37", "addx 13", "addx -6", "addx -2", "addx 5", "addx 25", "addx 2", "addx -24", "addx 2", "addx 5", "addx 5", "noop", "noop", "addx -2", "addx 2", "addx 5", "addx 2", "addx 7", "addx -2", "noop", "addx -8", "addx 9", "addx -36", "noop", "noop", "addx 5", "addx 6", "noop", "addx 25", "addx -24", "addx 3", "addx -2", "noop", "addx 3", "addx 6", "noop", "addx 9", "addx -8", "addx 5", "addx 2", "addx -7", "noop", "addx 12", "addx -10", "addx 11", "addx -38", "addx 22", "addx -15", "addx -3", "noop", "addx 32", "addx -25", "addx -7", "addx 11", "addx 5", "addx 10", "addx -9", "addx 17", "addx -12", "addx 2", "noop", "addx 2", "addx -15", "addx 22", "noop", "noop", "noop", "addx -35", "addx 7", "addx 21", "addx -25", "noop", "addx 3", "addx 2", "noop", "addx 7", "noop", "addx 3", "noop", "addx 2", "addx 9", "addx -4", "addx -2", "addx 5", "addx 2", "addx -2", "noop", "addx 7", "addx 2", "addx -39", "addx 2", "noop", "addx 1", "noop", "addx 5", "addx 24", "addx -20", "addx 1", "addx 5", "noop", "noop", "addx 4", "noop", "addx 1", "noop", "addx 4", "addx 3", "noop", "addx 2", "noop", "noop", "addx 1", "addx 2", "noop", "addx 3", "noop", "noop"];

console.log('sample part 1', part1(sample)); // 13140
console.log('input part 1', part1(input)); // 16020

console.log('sample part 2');

part2(sample);

/*
##..##..##..##..##..##..##..##..##..##..
###...###...###...###...###...###...###.
####....####....####....####....####....
#####.....#####.....#####.....#####.....
######......######......######......####
#######.......#######.......#######.....
 */

console.log('input part 2'); // ECZUZALR

part2(input);

/*
####..##..####.#..#.####..##..#....###..
#....#..#....#.#..#....#.#..#.#....#..#.
###..#......#..#..#...#..#..#.#....#..#.
#....#.....#...#..#..#...####.#....###..
#....#..#.#....#..#.#....#..#.#....#.#..
####..##..####..##..####.#..#.####.#..#.
 */
```
