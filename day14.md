```js
const input = ['498,4 -> 498,6 -> 496,6', '503,4 -> 502,4 -> 502,9 -> 494,9'];

const Element = {
    ROCK: '#',
    AIR: '.',
    SAND: 'o',
    SAND_SOURCE: '+'
}

function getRockPaths(input) {
    return input.map(line => {
        return line
            .split(' -> ')
            .map(pos => [Number(pos.split(',')[0]), Number(pos.split(',')[1])]);
    });
}

function getCaveSize(rockPaths) {
    const caveSize = rockPaths.reduce((acc, path) => {
        const pathMax = path.reduce((acc2, pos) => [Math.max(acc2[0], pos[0]), Math.max(acc2[1], pos[1])], [0, 0]);
        return [Math.max(acc[0], pathMax[0]), Math.max(acc[1], pathMax[1])];
    }, [0, 0]);

    return [caveSize[0]+1, caveSize[1]+1];
}

function getMinRockPos(rockPaths, caveSize) {
    const minRockPos = rockPaths.reduce((acc, path) => {
        const pathMin = path.reduce((acc2, pos) => [Math.min(acc2[0], pos[0]), Math.min(acc2[1], pos[1])], caveSize);
        return [Math.min(acc[0], pathMin[0]), Math.min(acc[1], pathMin[1])];
    }, caveSize);

    return [minRockPos[0], minRockPos[1]];
}

function createCaveMap(caveSize, rockPaths) {
    const caveMap = Array(caveSize[1]).fill(0).map(() => {return Array(caveSize[0]).fill(Element.AIR)});

    rockPaths.forEach((path) => {
        path.forEach((pos, pathIndex) => {
           const pos2 = path[pathIndex+1];

           if (pos && pos2) {
               const firstCol = pos[0];
               const secondCol = pos2[0];
               const firstRow = pos[1];
               const secondRow = pos2[1];

               if (firstCol < secondCol) {
                   for(let i=firstCol; i<=secondCol; i++) {
                       caveMap[firstRow][i] = Element.ROCK;
                   }
               } else if(secondCol < firstCol) {
                   for(let i=secondCol; i<=firstCol; i++) {
                       caveMap[firstRow][i] = Element.ROCK;
                   }
               } else if (firstRow < secondRow) {
                   for(let i=firstRow; i<=secondRow; i++) {
                       caveMap[i][firstCol] = Element.ROCK;
                   }
               } else if(secondRow < firstRow) {
                   for(let i=secondRow; i<=firstRow; i++) {
                       caveMap[i][firstCol] = Element.ROCK;
                   }
               }
           }
        });
    });

    caveMap[0][500] = Element.SAND_SOURCE;

    return caveMap;
}

function getMapAfterSandStep(caveMap, currentSandPos, nextSandPos) {
    // Space to clone caveMap ??

    caveMap[currentSandPos[0]][currentSandPos[1]] = Element.AIR;
    caveMap[nextSandPos[0]][nextSandPos[1]] = Element.SAND;
    caveMap[0][500] = Element.SAND_SOURCE;

    return caveMap;
}

function printMap(caveSize, minRockPos, caveMap) {
    for (let i=0; i<caveSize[1]; i++) {
        const row = caveMap[i];
        console.log(row.slice(minRockPos[0], caveSize[0]).join(''));
    }
}

const rockPaths = getRockPaths(input);

console.log(rockPaths);

const caveSize = getCaveSize(rockPaths);

console.log(caveSize);

const minRockPos = getMinRockPos(rockPaths, caveSize);

console.log(minRockPos);

let caveMap = createCaveMap(caveSize, rockPaths);

printMap(caveSize, minRockPos, caveMap);

for(let i=0; i<24; i++) {
    console.log('sand', i);
    let currentSandPos = [0, 500];
    let nextSandPos = [currentSandPos[0] + 1, currentSandPos[1]];
    let nextElement = caveMap[nextSandPos[0]][nextSandPos[1]];

    while (nextElement === Element.AIR) {
        caveMap = getMapAfterSandStep(caveMap, currentSandPos, nextSandPos);

        currentSandPos = [currentSandPos[0] + 1, currentSandPos[1]];
        nextSandPos = [nextSandPos[0] + 1, currentSandPos[1]];
        nextElement = caveMap[nextSandPos[0]][nextSandPos[1]];
    }

    printMap(caveSize, minRockPos, caveMap);
    console.log('checking diagonal');

    if (nextElement === Element.SAND || nextElement === Element.ROCK) {
        let diagonalLeftPos = [currentSandPos[0] + 1, currentSandPos[1] - 1];
        let diagonalRightPos = [currentSandPos[0] + 1, currentSandPos[1] + 1];

        let diagonalLeftElement = caveMap[diagonalLeftPos[0]][diagonalLeftPos[1]];
        let diagonalRightElement = caveMap[diagonalRightPos[0]][diagonalRightPos[1]];

        while (nextElement === Element.AIR || diagonalLeftElement === Element.AIR || diagonalRightElement === Element.AIR) {
            if(nextElement === Element.AIR) {
                console.log('down');
                caveMap = getMapAfterSandStep(caveMap, currentSandPos, nextSandPos);
                currentSandPos = [currentSandPos[0] + 1, currentSandPos[1]];
            } else if (diagonalLeftElement === Element.AIR) {
                console.log('left');
                caveMap = getMapAfterSandStep(caveMap, currentSandPos, diagonalLeftPos);
                currentSandPos = [diagonalLeftPos[0], diagonalLeftPos[1]];
            } else if (diagonalRightElement === Element.AIR) {
                console.log('right');
                caveMap = getMapAfterSandStep(caveMap, currentSandPos, diagonalRightPos);
                currentSandPos = [diagonalRightPos[0], diagonalRightPos[1]];
            }

            printMap(caveSize, minRockPos, caveMap);

            nextSandPos = [currentSandPos[0] + 1, currentSandPos[1]];
            diagonalLeftPos = [currentSandPos[0] + 1, currentSandPos[1] - 1];
            diagonalRightPos = [currentSandPos[0] + 1, currentSandPos[1] + 1];

            nextElement = caveMap[nextSandPos[0]][nextSandPos[1]];
            diagonalLeftElement = caveMap[diagonalLeftPos[0]][diagonalLeftPos[1]];
            diagonalRightElement = caveMap[diagonalRightPos[0]][diagonalRightPos[1]];
        }
    }

    console.log('last map');
    printMap(caveSize, minRockPos, caveMap);
}
```
