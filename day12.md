# Challenge #12: Hill Climbing Algorithm

Codepen: https://codepen.io/mamunoz-dev/pen/WNKXdqR?editors=0012

Resolved using Breadth First Search (BFS) algorithm:
```js
function createHeightsMap(input) {
    const alphabet = "abcdefghijklmnopqrstuvwxyz";
    const heightsMap = [];
    let startPos = [0,0];
    let endPos = [0,0];

    for (let i=0; i<input.length; i++) {
        heightsMap.push([]);

        for (let j=0; j < input[i].length; j++) {
            switch (input[i][j]) {
                case 'S': {
                    startPos = [i, j];
                    heightsMap[i].push(0);
                    break;
                }
                case 'E': {
                    endPos = [i, j];
                    heightsMap[i].push(25);
                    break;
                }
                default: {
                    heightsMap[i].push(alphabet.indexOf(input[i][j]))
                }
            }
        }
    }

    return { heightsMap, startPos, endPos };
}

function getValidAdjacents(heightsMap, currPos, reversed = false){
    const [row, col] = currPos;
    const currHeight = heightsMap[row][col];
    const adjacent = [];

    if (reversed) {
        const right = heightsMap[row].length>col+1 && currHeight <= heightsMap[row][col+1] + 1;
        const left = col-1 >= 0 && currHeight <= heightsMap[row][col-1] + 1;
        const down = heightsMap.length>row+1 && currHeight <= heightsMap[row+1][col] + 1;
        const up = row-1 >= 0 && currHeight <= heightsMap[row-1][col] + 1;

        if (right) {
            adjacent.push([row, col+1]);
        }
        if (left) {
            adjacent.push([row, col-1]);
        }
        if (down) {
            adjacent.push([row+1, col]);
        }
        if (up) {
            adjacent.push([row-1, col]);
        }
    }
    else {
        const right = heightsMap[row].length>col+1 && heightsMap[row][col+1] <= currHeight + 1;
        const left = col-1 >= 0 && heightsMap[row][col-1] <= currHeight + 1;
        const down = heightsMap.length>row+1 && heightsMap[row+1][col] <= currHeight + 1;
        const up = row-1 >= 0 && heightsMap[row-1][col] <= currHeight + 1;

        if (right) {
            adjacent.push([row, col+1]);
        }
        if (left) {
            adjacent.push([row, col-1]);
        }
        if (down) {
            adjacent.push([row+1, col]);
        }
        if (up) {
            adjacent.push([row-1, col]);
        }
    }

    return adjacent;
}

function breadthFirstSearch(heightsMap, startPos, endFunction, reversed = false){
    const queue = [];
    const visited = new Set();
    const steps = new Map();

    // Initialize all with start position
    queue.push(startPos);
    steps.set(startPos.toString(), 0);
    visited.add(startPos.toString());

    while(queue.length > 0) {
        const currPos = queue.shift();
        const currPosKey = currPos.toString();

        if (endFunction(currPos)) {
            return steps.get(currPosKey);
        }

        const neighbours = getValidAdjacents(heightsMap, currPos, reversed);
        for (const adjacent of neighbours) {
            const adjacentPosKey = adjacent.toString();
            const adjacentVisited = visited.has(adjacentPosKey);
            const numberOfStepsIsShorter = steps.get(currPosKey)+1 < steps.get(adjacentPosKey);

            if (!adjacentVisited || numberOfStepsIsShorter) {
                steps.set(adjacentPosKey, steps.get(currPosKey)+1);
                queue.push(adjacent);
                visited.add(adjacentPosKey);
            }
        }

        //console.log('Queue:', queue);
        //console.log('Visited:', JSON.stringify(Array.from(visited)));
        //console.log('Steps:', steps);
    }
}

function part1(input) {
    const { heightsMap, startPos, endPos } = createHeightsMap(input);
    return breadthFirstSearch(heightsMap, startPos, (currPos) => currPos[0] === endPos[0] && currPos[1] === endPos[1]);
}

function part2(input) {
    const { heightsMap, endPos } = createHeightsMap(input);
    return breadthFirstSearch(heightsMap, endPos, (currPos) => heightsMap[currPos[0]][currPos[1]] === 0, true);
}

const sample = ["Sabqponm", "abcryxxl", "accszExk", "acctuvwj", "abdefghi"];
const input = ["abaacccccccccccccaaaaaaaccccccccccccccccccccccccccccccccccaaaaaa", "abaaccccccccccccccaaaaaaaaaaccccccccccccccccccccccccccccccccaaaa", "abaaaaacccccccccaaaaaaaaaaaaccccccccccccccccccccccccccccccccaaaa", "abaaaaaccccccccaaaaaaaaaaaaaacccccccccccccccccdcccccccccccccaaaa", "abaaaccccccccccaaaaaaaaccacacccccccccccccccccdddcccccccccccaaaaa", "abaaacccccccccaaaaaaaaaaccaaccccccccccccciiiiddddcccccccccccaccc", "abcaaaccccccccaaaaaaaaaaaaaaccccccccccciiiiiijddddcccccccccccccc", "abccaaccccccccaccaaaaaaaaaaaacccccccccciiiiiijjddddccccaaccccccc", "abccccccccccccccaaacaaaaaaaaaaccccccciiiiippijjjddddccaaaccccccc", "abccccccccccccccaacccccaaaaaaacccccciiiippppppjjjdddddaaaaaacccc", "abccccccccccccccccccccaaaaaaccccccckiiippppppqqjjjdddeeeaaaacccc", "abccccccccccccccccccccaaaaaaccccckkkiippppuupqqjjjjdeeeeeaaccccc", "abccccccccccccccccccccccccaaccckkkkkkipppuuuuqqqjjjjjeeeeeaccccc", "abccccccccccccccccccccccccccckkkkkkoppppuuuuuvqqqjjjjjkeeeeccccc", "abcccccccccccccccccccccccccckkkkooooppppuuxuvvqqqqqqjkkkeeeecccc", "abccaaccaccccccccccccccccccckkkoooooopuuuuxyvvvqqqqqqkkkkeeecccc", "abccaaaaacccccaaccccccccccckkkoooouuuuuuuxxyyvvvvqqqqqkkkkeecccc", "abcaaaaacccccaaaacccccccccckkkooouuuuxxxuxxyyvvvvvvvqqqkkkeeeccc", "abcaaaaaaaaaaaaacccccccccccjjjooottuxxxxxxxyyyyyvvvvrrrkkkeecccc", "abcccaaaacaaaaaaaaacaaccccccjjoootttxxxxxxxyyyyyyvvvrrkkkfffcccc", "SbccaacccccaaaaaaaaaaaccccccjjjooottxxxxEzzzyyyyvvvrrrkkkfffcccc", "abcccccccccaaaaaaaaaaaccccccjjjooootttxxxyyyyyvvvvrrrkkkfffccccc", "abcaacccccaaaaaaaaaaaccccccccjjjooottttxxyyyyywwvrrrrkkkfffccccc", "abaaacccccaaaaaaaaaaaaaacccccjjjjonnttxxyyyyyywwwrrlllkfffcccccc", "abaaaaaaaaaaacaaaaaaaaaaccccccjjjnnnttxxyywwyyywwrrlllffffcccccc", "abaaaaaaaaaaaaaaaaaaaaaaccccccjjjnntttxxwwwwwywwwrrlllfffccccccc", "abaaccaaaaaaaaaaaaaaacccccccccjjjnntttxwwwsswwwwwrrlllfffccccccc", "abaacccaaaaaaaacccaaacccccccccjjinnttttwwsssswwwsrrlllgffacccccc", "abccccaaaaaaccccccaaaccccccccciiinnntttsssssssssssrlllggaacccccc", "abccccaaaaaaaccccccccccaaccccciiinnntttsssmmssssssrlllggaacccccc", "abccccaacaaaacccccccaacaaaccccciinnnnnnmmmmmmmsssslllgggaaaacccc", "abccccccccaaacccccccaaaaacccccciiinnnnnmmmmmmmmmmllllgggaaaacccc", "abaaaccccccccccccccccaaaaaacccciiiinnnmmmhhhmmmmmlllgggaaaaccccc", "abaaaaacccccccccccaaaaaaaaaccccciiiiiiihhhhhhhhmmlgggggaaacccccc", "abaaaaaccccaaccccaaaaaaacaacccccciiiiihhhhhhhhhhggggggcaaacccccc", "abaaaaccccaaaccccaaaacaaaaacccccccciiihhaaaaahhhhggggccccccccccc", "abaaaaaaacaaacccccaaaaaaaaaccccccccccccccaaaacccccccccccccccccaa", "abaacaaaaaaaaaaaccaaaaaaaaccccccccccccccccaaaccccccccccccccccaaa", "abcccccaaaaaaaaacccaaaaaaaccccccccccccccccaacccccccccccccccccaaa", "abccccccaaaaaaaaaaaaaaaaacccccccccccccccccaaacccccccccccccaaaaaa", "abcccccaaaaaaaaaaaaaaaaaaaaaccccccccccccccccccccccccccccccaaaaaa"];

console.log("sample part 1:", part1(sample)); // 31
console.log("sample part 2:", part2(sample)); // 29

console.log("input part 1:", part1(input)); // 370
console.log("input part 2:", part2(input)); // 363
```
