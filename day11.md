```js
const sample = [{items:[79,98],opp:old=>old*19,base:23,next:[3,2],inspected:0},{items:[54,65,75,74],opp:old=>old+6,base:19,next:[0,2],inspected:0},{items:[79,60,97],opp:old=>old*old,base:13,next:[3,1],inspected:0},{items:[74],opp:old=>old+3,base:17,next:[1,0],inspected:0}];
const sample2 = [{items:[79,98],opp:old=>old*19,base:23,next:[3,2],inspected:0},{items:[54,65,75,74],opp:old=>old+6,base:19,next:[0,2],inspected:0},{items:[79,60,97],opp:old=>old*old,base:13,next:[3,1],inspected:0},{items:[74],opp:old=>old+3,base:17,next:[1,0],inspected:0}];
const input = [{items:[98,70,75,80,84,89,55,98],opp:old=>old*2,base:11,next:[4,1],inspected:0},{items:[59],opp:old=>old*old,base:19,next:[3,7],inspected:0},{items:[77,95,54,65,89],opp:old=>old+6,base:7,next:[5,0],inspected:0},{items:[71,64,75],opp:old=>old+2,base:17,next:[2,6],inspected:0},{items:[74,55,87,98],opp:old=>old*11,base:3,next:[7,1],inspected:0},{items:[90,98,85,52,91,60],opp:old=>old+7,base:5,next:[4,0],inspected:0},{items:[99,51],opp:old=>old+1,base:13,next:[2,5],inspected:0},{items:[98,94,59,76,51,65,75],opp:old=>old+5,base:2,next:[6,3],inspected:0}];
const input2 = [{items:[98,70,75,80,84,89,55,98],opp:old=>old*2,base:11,next:[4,1],inspected:0},{items:[59],opp:old=>old*old,base:19,next:[3,7],inspected:0},{items:[77,95,54,65,89],opp:old=>old+6,base:7,next:[5,0],inspected:0},{items:[71,64,75],opp:old=>old+2,base:17,next:[2,6],inspected:0},{items:[74,55,87,98],opp:old=>old*11,base:3,next:[7,1],inspected:0},{items:[90,98,85,52,91,60],opp:old=>old+7,base:5,next:[4,0],inspected:0},{items:[99,51],opp:old=>old+1,base:13,next:[2,5],inspected:0},{items:[98,94,59,76,51,65,75],opp:old=>old+5,base:2,next:[6,3],inspected:0}];

function test(val, base) {
    return val % base === 0;
}

function playRound(monkeys, totalRounds) {
    const base = monkeys.reduce((acc, monkey) => acc * monkey.base, 1);

    for (const monkey of monkeys) {
        while (monkey.items.length > 0) {
            let worryLevel = monkey.opp(monkey.items.shift());
            worryLevel = totalRounds <= 20 ? Math.floor(worryLevel / 3) : worryLevel % base;

            const nextMonkey = monkey.next[+ test(worryLevel, monkey.base)];
            monkeys[nextMonkey].items.push(worryLevel);
            monkey.inspected++;
        }
    }

    return monkeys;
}

function playGame(monkeys, totalRounds) {
    for(let i=0; i<totalRounds; i++) {
        monkeys = playRound(monkeys, totalRounds);
    }

    const monkeysSortedByInspectedItems = monkeys.sort((a, b) => b.inspected - a.inspected);
    return monkeysSortedByInspectedItems[0].inspected * monkeysSortedByInspectedItems[1].inspected;
}

function part1(input) {
    return playGame(input, 20);
}

function part2(input) {
    return playGame(input, 10000);
}

console.log('sample part 1', part1(sample, 20)); // 10605
console.log('sample part 2', part2(sample2, 10000)); // 2713310158
console.log('input part 1', part1(input, 20)); // 54253
console.log('input part 2', part2(input2, 10000)); // 13119526120
```
