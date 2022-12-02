### Part 1

```js
const MoveType = {
	ROCK: 'Rock',
  PAPER: 'Paper',
  SCISSORS: 'Scissors'
}

const moves = {
	A: { name: MoveType.ROCK, value: 1 },
  B: { name: MoveType.PAPER, value: 2 },
  C: { name: MoveType.SCISSORS, value: 3 },
  X: { name: MoveType.ROCK, value: 1 },
  Y: { name: MoveType.PAPER, value: 2 },
  Z: { name: MoveType.SCISSORS, value: 3 }
};

const outcomes = {
	WIN: 6,
  DRAW: 3,
  LOSE: 0
};

function handResult(elfsHand, myHand) {
	const elfsHandValue = moves[elfsHand].value;
  const myHandsValue = moves[myHand].value;
  const elfsHandName = moves[elfsHand].name;
  const myHandsName = moves[myHand].name;
  let outcome = outcomes.DRAW;
  
  
  if (myHandsName === MoveType.ROCK && elfsHandName === MoveType.SCISSORS || myHandsName === MoveType.SCISSORS && elfsHandName === MoveType.PAPER || myHandsName === MoveType.PAPER && elfsHandName === MoveType.ROCK) {
  	outcome = outcomes.WIN;
  } else if (myHandsName === MoveType.SCISSORS && elfsHandName === MoveType.ROCK || myHandsName === MoveType.PAPER && elfsHandName === MoveType.SCISSORS || myHandsName === MoveType.ROCK && elfsHandName === MoveType.PAPER) {
  	outcome = outcomes.LOSE;
  }
  
  // console.log(moves[myHand].name, moves[elfsHand].name, myHandsValue, outcome, 'Points:', outcome + myHandsValue);
  
  return outcome + myHandsValue;
}

function game(hands) {
	return hands.reduce((acc, currentHand) => {
  	const currentHandArray = currentHand.split(' ');
    const elfsHand = currentHandArray[0];
    const myHand = currentHandArray[1];
    
    return acc + handResult(elfsHand, myHand);
  }, 0);
}

const hands = ["A Y", "B X", "C Z"];

console.log(game(hands));
```

### Part 2 (Refactor, for godness sake)

```js
const MoveType = {
	ROCK: 'Rock',
  PAPER: 'Paper',
  SCISSORS: 'Scissors'
}

const outcomes = {
	WIN: 6,
  DRAW: 3,
  LOSE: 0
};

const moves = {
	A: { name: MoveType.ROCK, value: 1 },
  B: { name: MoveType.PAPER, value: 2 },
  C: { name: MoveType.SCISSORS, value: 3 },
  X: { name: outcomes.LOSE },
  Y: { name: outcomes.DRAW },
  Z: { name: outcomes.WIN }
};

const values = {
	'Rock': 1,
  'Paper': 2,
  'Scissors': 3
}

function decideHand(outcome, elfsHandName) {
	let myHandsName = elfsHandName;
  
  if(outcome === outcomes.WIN) {
  	if (elfsHandName === MoveType.SCISSORS) {
    	myHandsName = MoveType.ROCK;
    } else if (elfsHandName === MoveType.PAPER) {
    	myHandsName = MoveType.SCISSORS;
    } else if (elfsHandName === MoveType.ROCK) {
    	myHandsName = MoveType.PAPER;
    }
  } else if (outcome === outcomes.LOSE) {
  	if (elfsHandName === MoveType.ROCK) {
    	myHandsName = MoveType.SCISSORS;
    } else if (elfsHandName === MoveType.SCISSORS) {
    	myHandsName = MoveType.PAPER;
    } else if (elfsHandName === MoveType.PAPER) {
    	myHandsName = MoveType.ROCK;
    }
  }
  
  return myHandsName;
}

function handResult(elfsHand, myHand) {
	const elfsHandValue = moves[elfsHand].value;
  const elfsHandName = moves[elfsHand].name;
  const outcome = moves[myHand].name;
  
  const myHandsName = decideHand(outcome, elfsHandName);
  
  return outcome + values[myHandsName];
}

function game(hands) {
	return hands.reduce((acc, currentHand) => {
  	const currentHandArray = currentHand.split(' ');
    const elfsHand = currentHandArray[0];
    const myHand = currentHandArray[1];
    
    return acc + handResult(elfsHand, myHand);
  }, 0);
}

const hands = ["A Y", "B X", "C Z"];

console.log(game(hands));
```
