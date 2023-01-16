# Day 5: Supply Stacks

Codepen: https://codepen.io/mamunoz-dev/pen/KKBvEod?editors=0012

```js
function formatMove(move) {
  const simplifiedMoves = move.replace('move ', '').replace(' from', '').replace(' to', '').split(' ');
  return { numberOfBoxes: simplifiedMoves[0], orgStack: simplifiedMoves[1], dstStack: simplifiedMoves[2] };
}

function formatMoves(moves) {
  return moves.map((move) => formatMove(move));
}

function getTopMostBoxOfEachStack(stacks) {
  return Object.values(stacks).reduce((acc, stack) => acc + stack[stack.length-1], '');
}

function organizeStacksPart1(stacks, moves) {
  const formattedMoves = formatMoves(moves);

  formattedMoves.forEach((move) => {
    for(let i=0; i<move.numberOfBoxes; i++) {
      const poppedElement = stacks[move.orgStack].pop();
      stacks[move.dstStack].push(poppedElement);
    }
  });

  return stacks;
}

function organizeStacksPart2(stacks, moves) {
  const formattedMoves = formatMoves(moves);

  formattedMoves.forEach((move) => {
    const poppedElements = [];

    for(let i=0; i<move.numberOfBoxes; i++) {
      const poppedElement = stacks[move.orgStack].pop();
      poppedElements.unshift(poppedElement);
    }

    poppedElements.forEach(elm => {
      stacks[move.dstStack].push(elm);
    });
  });

  return stacks;
}

function part1(inputStacks, inputMoves) {
  let stacksCopy = JSON.parse(JSON.stringify(inputStacks));
  stacksCopy = organizeStacksPart1(stacksCopy, inputMoves);
  return getTopMostBoxOfEachStack(stacksCopy);
}

function part2(inputStacks, inputMoves) {
  let stacksCopy = JSON.parse(JSON.stringify(inputStacks));
  stacksCopy = organizeStacksPart2(stacksCopy, inputMoves);
  return getTopMostBoxOfEachStack(stacksCopy);
}

const sampleStacks = {
  1: 'ZN'.split(''),
  2: 'MCD'.split(''),
  3: 'P'.split('')
}

const sampleMoves = ["move 1 from 2 to 1", "move 3 from 1 to 3", "move 2 from 2 to 1", "move 1 from 1 to 2"];

const inputStacks = {
  1: 'NBDTVGZJ'.split(''),
  2: 'SRMDWPF'.split(''),
  3: 'VCRSZ'.split(''),
  4: 'RTJZPHG'.split(''),
  5: 'TCJNDZQF'.split(''),
  6: 'NVPWGSFM'.split(''),
  7: 'GCVBPQ'.split(''),
  8: 'ZBPN'.split(''),
  9: 'WPJ'.split(''),
};

const inputMoves = ["move 2 from 4 to 6", "move 1 from 9 to 5", "move 3 from 2 to 4", "move 8 from 4 to 7", "move 2 from 9 to 7", "move 3 from 8 to 3", "move 2 from 1 to 2", "move 5 from 7 to 9", "move 1 from 9 to 4", "move 1 from 8 to 3", "move 1 from 3 to 4", "move 2 from 4 to 9", "move 7 from 3 to 5", "move 6 from 1 to 8", "move 11 from 7 to 9", "move 12 from 5 to 3", "move 6 from 6 to 9", "move 3 from 3 to 8", "move 4 from 2 to 7", "move 3 from 5 to 7", "move 1 from 5 to 7", "move 2 from 2 to 5", "move 1 from 5 to 2", "move 5 from 8 to 9", "move 7 from 7 to 2", "move 3 from 8 to 7", "move 1 from 8 to 9", "move 4 from 3 to 6", "move 1 from 5 to 1", "move 9 from 9 to 6", "move 7 from 9 to 6", "move 20 from 6 to 5", "move 12 from 9 to 8", "move 5 from 5 to 1", "move 3 from 7 to 4", "move 6 from 2 to 7", "move 2 from 3 to 1", "move 4 from 3 to 8", "move 1 from 4 to 1", "move 7 from 7 to 5", "move 4 from 8 to 2", "move 3 from 6 to 2", "move 3 from 2 to 9", "move 4 from 1 to 7", "move 2 from 1 to 2", "move 3 from 9 to 5", "move 11 from 8 to 5", "move 1 from 6 to 9", "move 1 from 8 to 5", "move 1 from 1 to 2", "move 24 from 5 to 4", "move 2 from 1 to 6", "move 11 from 5 to 4", "move 2 from 7 to 9", "move 1 from 6 to 2", "move 4 from 2 to 1", "move 28 from 4 to 2", "move 1 from 7 to 8", "move 9 from 2 to 5", "move 2 from 9 to 6", "move 4 from 4 to 2", "move 1 from 7 to 4", "move 3 from 4 to 7", "move 1 from 6 to 9", "move 21 from 2 to 3", "move 3 from 1 to 6", "move 5 from 6 to 2", "move 7 from 2 to 3", "move 1 from 9 to 3", "move 1 from 8 to 4", "move 1 from 7 to 8", "move 3 from 5 to 8", "move 1 from 1 to 7", "move 2 from 7 to 9", "move 2 from 8 to 4", "move 1 from 9 to 2", "move 1 from 8 to 6", "move 11 from 3 to 4", "move 1 from 7 to 8", "move 6 from 5 to 9", "move 2 from 8 to 7", "move 1 from 6 to 5", "move 7 from 3 to 8", "move 9 from 3 to 6", "move 1 from 8 to 3", "move 1 from 7 to 4", "move 2 from 3 to 5", "move 4 from 5 to 7", "move 4 from 6 to 8", "move 2 from 7 to 9", "move 11 from 4 to 2", "move 1 from 4 to 2", "move 6 from 8 to 9", "move 1 from 7 to 1", "move 1 from 3 to 7", "move 3 from 7 to 8", "move 6 from 8 to 9", "move 6 from 4 to 8", "move 18 from 9 to 3", "move 1 from 5 to 8", "move 5 from 6 to 5", "move 6 from 8 to 1", "move 3 from 5 to 4", "move 1 from 9 to 8", "move 3 from 4 to 8", "move 15 from 3 to 6", "move 2 from 5 to 9", "move 3 from 3 to 1", "move 9 from 6 to 4", "move 2 from 1 to 5", "move 2 from 5 to 8", "move 6 from 4 to 2", "move 6 from 1 to 6", "move 3 from 4 to 6", "move 6 from 9 to 1", "move 4 from 2 to 1", "move 7 from 8 to 1", "move 1 from 6 to 7", "move 17 from 1 to 5", "move 1 from 7 to 1", "move 5 from 2 to 1", "move 1 from 8 to 6", "move 11 from 6 to 4", "move 2 from 2 to 3", "move 3 from 1 to 8", "move 7 from 2 to 5", "move 4 from 6 to 7", "move 4 from 1 to 5", "move 15 from 5 to 9", "move 2 from 3 to 7", "move 2 from 8 to 2", "move 1 from 1 to 9", "move 6 from 2 to 6", "move 7 from 5 to 6", "move 5 from 7 to 3", "move 1 from 6 to 1", "move 2 from 3 to 4", "move 1 from 3 to 4", "move 5 from 6 to 4", "move 14 from 9 to 2", "move 1 from 8 to 9", "move 1 from 7 to 8", "move 1 from 9 to 6", "move 2 from 9 to 5", "move 1 from 1 to 2", "move 7 from 6 to 9", "move 1 from 3 to 4", "move 8 from 5 to 2", "move 1 from 6 to 7", "move 1 from 7 to 4", "move 1 from 8 to 4", "move 1 from 3 to 9", "move 7 from 9 to 5", "move 1 from 9 to 1", "move 6 from 5 to 1", "move 8 from 2 to 4", "move 1 from 5 to 6", "move 1 from 6 to 7", "move 1 from 7 to 9", "move 7 from 2 to 9", "move 1 from 9 to 4", "move 3 from 9 to 1", "move 1 from 9 to 6", "move 11 from 2 to 8", "move 9 from 1 to 8", "move 1 from 6 to 4", "move 1 from 1 to 9", "move 12 from 4 to 2", "move 4 from 9 to 3", "move 3 from 4 to 6", "move 9 from 8 to 6", "move 12 from 4 to 9", "move 8 from 6 to 3", "move 8 from 2 to 7", "move 11 from 3 to 4", "move 2 from 2 to 7", "move 2 from 6 to 1", "move 1 from 2 to 3", "move 2 from 6 to 2", "move 3 from 2 to 6", "move 2 from 1 to 6", "move 1 from 6 to 1", "move 1 from 6 to 4", "move 2 from 6 to 3", "move 1 from 6 to 5", "move 4 from 3 to 8", "move 12 from 4 to 5", "move 5 from 9 to 7", "move 3 from 8 to 7", "move 1 from 9 to 1", "move 3 from 8 to 2", "move 13 from 5 to 6", "move 1 from 2 to 9", "move 13 from 6 to 7", "move 7 from 9 to 6", "move 2 from 4 to 6", "move 1 from 8 to 6", "move 1 from 1 to 6", "move 1 from 2 to 9", "move 1 from 2 to 3", "move 12 from 7 to 9", "move 7 from 8 to 4", "move 1 from 1 to 3", "move 2 from 7 to 9", "move 15 from 7 to 4", "move 8 from 6 to 3", "move 1 from 8 to 9", "move 1 from 7 to 2", "move 10 from 3 to 5", "move 6 from 5 to 9", "move 1 from 2 to 8", "move 1 from 5 to 8", "move 2 from 8 to 9", "move 10 from 4 to 9", "move 20 from 9 to 6", "move 1 from 7 to 6", "move 4 from 9 to 3", "move 1 from 5 to 9", "move 4 from 4 to 9", "move 8 from 9 to 7", "move 2 from 5 to 1", "move 7 from 4 to 3", "move 8 from 3 to 2", "move 6 from 9 to 8", "move 1 from 3 to 7", "move 1 from 3 to 1", "move 7 from 7 to 8", "move 13 from 8 to 3", "move 2 from 2 to 8", "move 1 from 8 to 2", "move 1 from 4 to 1", "move 1 from 1 to 8", "move 2 from 8 to 2", "move 24 from 6 to 2", "move 2 from 7 to 8", "move 5 from 3 to 4", "move 25 from 2 to 6", "move 5 from 4 to 9", "move 2 from 8 to 7", "move 2 from 7 to 3", "move 4 from 6 to 2", "move 2 from 6 to 4", "move 9 from 2 to 3", "move 11 from 3 to 7", "move 10 from 7 to 8", "move 1 from 7 to 9", "move 3 from 2 to 4", "move 8 from 8 to 2", "move 1 from 2 to 6", "move 2 from 4 to 1", "move 1 from 8 to 2", "move 1 from 6 to 9", "move 1 from 8 to 3", "move 6 from 9 to 7", "move 2 from 9 to 1", "move 9 from 6 to 8", "move 7 from 2 to 3", "move 7 from 8 to 2", "move 10 from 6 to 8", "move 7 from 1 to 2", "move 9 from 3 to 2", "move 5 from 3 to 8", "move 4 from 7 to 2", "move 2 from 3 to 2", "move 12 from 2 to 3", "move 6 from 4 to 2", "move 1 from 7 to 6", "move 5 from 3 to 5", "move 16 from 8 to 4", "move 12 from 2 to 7", "move 5 from 5 to 7", "move 1 from 8 to 3", "move 1 from 6 to 4", "move 17 from 7 to 4", "move 1 from 7 to 1", "move 1 from 1 to 9", "move 1 from 9 to 5", "move 11 from 4 to 9", "move 10 from 2 to 3", "move 1 from 5 to 4", "move 1 from 9 to 2", "move 2 from 2 to 1", "move 1 from 2 to 3", "move 23 from 4 to 5", "move 7 from 9 to 7", "move 3 from 9 to 1", "move 20 from 5 to 6", "move 3 from 5 to 8", "move 1 from 4 to 1", "move 2 from 8 to 3", "move 4 from 6 to 4", "move 7 from 7 to 2", "move 1 from 8 to 4", "move 19 from 3 to 9", "move 5 from 1 to 7", "move 7 from 2 to 6", "move 3 from 7 to 5", "move 2 from 3 to 4", "move 1 from 5 to 4", "move 1 from 1 to 4", "move 1 from 7 to 6", "move 13 from 6 to 7", "move 6 from 9 to 3", "move 1 from 3 to 5", "move 2 from 3 to 4", "move 2 from 6 to 2", "move 3 from 4 to 3", "move 8 from 9 to 1", "move 2 from 2 to 1", "move 8 from 6 to 7", "move 2 from 9 to 4", "move 20 from 7 to 1", "move 2 from 7 to 5", "move 2 from 5 to 1", "move 8 from 1 to 8", "move 8 from 8 to 6", "move 1 from 6 to 9", "move 8 from 6 to 1", "move 1 from 5 to 3", "move 7 from 3 to 2", "move 1 from 5 to 2", "move 2 from 9 to 7", "move 1 from 5 to 8", "move 18 from 1 to 4", "move 1 from 8 to 9", "move 3 from 2 to 3", "move 2 from 7 to 4", "move 5 from 2 to 4", "move 3 from 3 to 8", "move 8 from 1 to 7", "move 2 from 9 to 2", "move 32 from 4 to 5", "move 1 from 9 to 7", "move 1 from 2 to 1", "move 6 from 1 to 6", "move 1 from 2 to 4", "move 3 from 8 to 1", "move 3 from 6 to 5", "move 1 from 3 to 6", "move 2 from 1 to 9", "move 4 from 4 to 7", "move 31 from 5 to 4", "move 4 from 5 to 6", "move 1 from 6 to 1", "move 7 from 6 to 5", "move 1 from 9 to 4", "move 19 from 4 to 2", "move 1 from 5 to 9", "move 5 from 5 to 6", "move 3 from 4 to 2", "move 2 from 7 to 1", "move 4 from 7 to 8", "move 3 from 8 to 6", "move 2 from 6 to 7", "move 6 from 7 to 8", "move 3 from 1 to 5", "move 4 from 5 to 9", "move 15 from 2 to 1", "move 4 from 6 to 4", "move 2 from 6 to 3", "move 1 from 3 to 7", "move 4 from 1 to 2", "move 1 from 3 to 4", "move 2 from 7 to 4", "move 5 from 9 to 3", "move 2 from 7 to 3", "move 16 from 4 to 8", "move 8 from 8 to 5", "move 2 from 1 to 5", "move 1 from 9 to 6", "move 1 from 6 to 5", "move 7 from 5 to 9", "move 3 from 1 to 8", "move 1 from 8 to 4", "move 8 from 2 to 7", "move 3 from 1 to 3", "move 1 from 3 to 9", "move 2 from 4 to 2", "move 7 from 8 to 5", "move 7 from 9 to 1", "move 6 from 3 to 5", "move 6 from 7 to 4", "move 3 from 4 to 1", "move 3 from 2 to 5", "move 1 from 7 to 8", "move 1 from 7 to 5", "move 1 from 9 to 8", "move 2 from 2 to 4", "move 15 from 1 to 6", "move 8 from 5 to 9", "move 3 from 3 to 4", "move 4 from 4 to 3", "move 1 from 9 to 7", "move 6 from 9 to 4", "move 1 from 9 to 2", "move 6 from 4 to 9", "move 2 from 4 to 6", "move 5 from 6 to 9", "move 1 from 3 to 1", "move 8 from 6 to 8", "move 12 from 5 to 3", "move 1 from 5 to 3", "move 1 from 3 to 8", "move 4 from 6 to 1", "move 11 from 3 to 8", "move 1 from 2 to 1", "move 23 from 8 to 2", "move 3 from 1 to 2", "move 1 from 1 to 9", "move 2 from 2 to 3", "move 6 from 3 to 6", "move 1 from 7 to 6", "move 1 from 4 to 7", "move 1 from 4 to 3", "move 1 from 7 to 3", "move 4 from 8 to 4", "move 2 from 1 to 8", "move 3 from 8 to 1", "move 4 from 6 to 2", "move 7 from 9 to 1", "move 1 from 9 to 6", "move 2 from 2 to 3", "move 3 from 9 to 4", "move 1 from 9 to 3", "move 10 from 2 to 8", "move 16 from 2 to 5", "move 2 from 3 to 6", "move 6 from 1 to 8", "move 1 from 1 to 5", "move 8 from 8 to 5", "move 11 from 5 to 9", "move 2 from 1 to 8", "move 1 from 1 to 8", "move 4 from 4 to 6", "move 3 from 3 to 9", "move 14 from 9 to 3", "move 15 from 8 to 5", "move 9 from 5 to 4", "move 7 from 6 to 1", "move 1 from 6 to 3", "move 4 from 4 to 7", "move 2 from 6 to 2", "move 4 from 7 to 4", "move 4 from 1 to 4", "move 10 from 4 to 3", "move 14 from 3 to 6", "move 5 from 4 to 1", "move 6 from 5 to 7", "move 1 from 2 to 6", "move 3 from 7 to 2", "move 2 from 2 to 3", "move 3 from 7 to 8", "move 2 from 8 to 2", "move 2 from 2 to 7", "move 6 from 6 to 2", "move 1 from 8 to 7", "move 8 from 2 to 7", "move 1 from 4 to 1", "move 5 from 5 to 3", "move 3 from 3 to 2", "move 5 from 1 to 3", "move 7 from 5 to 8", "move 6 from 6 to 3", "move 1 from 5 to 9", "move 10 from 7 to 9", "move 26 from 3 to 4", "move 1 from 5 to 1", "move 6 from 8 to 2", "move 9 from 2 to 9", "move 1 from 7 to 5", "move 1 from 8 to 5", "move 2 from 6 to 2", "move 20 from 9 to 6", "move 1 from 1 to 6", "move 1 from 4 to 2", "move 1 from 5 to 8", "move 1 from 5 to 7", "move 3 from 1 to 3", "move 1 from 3 to 6", "move 12 from 4 to 8", "move 11 from 4 to 5", "move 1 from 7 to 5", "move 1 from 2 to 8", "move 1 from 1 to 8", "move 2 from 2 to 5", "move 8 from 6 to 2", "move 5 from 6 to 4", "move 2 from 5 to 3", "move 12 from 8 to 4", "move 5 from 2 to 6", "move 3 from 8 to 1", "move 11 from 6 to 8", "move 10 from 4 to 6", "move 5 from 4 to 6", "move 12 from 6 to 5", "move 22 from 5 to 6", "move 3 from 6 to 5", "move 3 from 8 to 5", "move 1 from 3 to 8", "move 4 from 8 to 1", "move 6 from 1 to 7", "move 5 from 6 to 9"];

console.log('part 1 sample', part1(sampleStacks, sampleMoves)); // CMZ
console.log('part 1 input', part1(inputStacks, inputMoves)); // GFTNRBZPF
console.log('part 2 sample', part2(sampleStacks, sampleMoves)); // MCD
console.log('part 2 input', part2(inputStacks, inputMoves)); // VRQWPDSGP
```
