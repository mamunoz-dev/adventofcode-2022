# Day 16: Proboscidea Volcanium

Codepen: https://codepen.io/mamunoz-dev/pen/mdjqGZd?editors=0012

Resolved using Floyd Warshall algorithm to initialize shortest path (time) matrix + Depth First Search (DFS) + Bitmask:
```js
function parseValvesData(input) {
  return input
    .map(line => /Valve (\w+) has flow rate=(\d+); tunnels? leads? to valves? (.+)/g.exec(line))
    .map(([_, valve, rate, to]) => ({valve, rate: Number(rate), to: to.split(", "),}))
    .reduce((acc, { valve, rate, to }) => Object.assign(acc, { [valve]: { rate, to } }), {})
}

function prepareData(input) {
  const valvesAndFlowRates = parseValvesData(input)
  const valves = Object.keys(valvesAndFlowRates);
  const valvesHavingPositiveRate = valves.filter(valve => Boolean(valvesAndFlowRates[valve].rate)); // We're not interested in valves with flow rate 0
  const valveBitRepr =
    valvesHavingPositiveRate.reduce((acc, valve, i) => Object.assign(acc, { [valve]: 1 << i }), {});

  /*
  valvesAndFlowRates = {
    AA: { rate: 0, to: [ 'DD', 'II', 'BB' ] },
    BB: { rate: 13, to: [ 'CC', 'AA' ] },
    CC: { rate: 2, to: [ 'DD', 'BB' ] },
    DD: { rate: 20, to: [ 'CC', 'AA', 'EE' ] },
    EE: { rate: 3, to: [ 'FF', 'DD' ] },
    FF: { rate: 0, to: [ 'EE', 'GG' ] },
    GG: { rate: 0, to: [ 'FF', 'HH' ] },
    HH: { rate: 22, to: [ 'GG' ] },
    II: { rate: 0, to: [ 'AA', 'JJ' ] },
    JJ: { rate: 21, to: [ 'II' ] }
  }
  */
  // valves = [ 'AA', 'BB', 'CC', 'DD', 'EE', 'FF', 'GG', 'HH', 'II', 'JJ' ]
  // valvesHavingPositiveRate = [ 'BB', 'CC', 'DD', 'EE', 'HH', 'JJ' ]
  // valveBitRepr = { BB: 1 (000001), CC: 2 (000010), DD: 4 (000100), EE: 8 (001000), HH: 16 (010000), JJ: 32 (100000)}

  const shortestPathMatrix = getFloydWarshallShortestPathMatrix(valves, valvesAndFlowRates);

  /*
  shortestPathMatrix = {
    AA: { AA: 2, BB: 1, CC: 2, DD: 1, EE: 2, FF: 3, GG: 4, HH: 5, II: 1, JJ: 2 },
    BB: { AA: 1, BB: 2, CC: 1, DD: 2, EE: 3, FF: 4, GG: 5, HH: 6, II: 2, JJ: 3 },
    CC: { AA: 2, BB: 1, CC: 2, DD: 1, EE: 2, FF: 3, GG: 4, HH: 5, II: 3, JJ: 4 },
    DD: { AA: 1, BB: 2, CC: 1, DD: 2, EE: 1, FF: 2, GG: 3, HH: 4, II: 2, JJ: 3 },
    EE: { AA: 2, BB: 3, CC: 2, DD: 1, EE: 2, FF: 1, GG: 2, HH: 3, II: 3, JJ: 4 },
    FF: { AA: 3, BB: 4, CC: 3, DD: 2, EE: 1, FF: 2, GG: 1, HH: 2, II: 4, JJ: 5 },
    GG: { AA: 4, BB: 5, CC: 4, DD: 3, EE: 2, FF: 1, GG: 2, HH: 1, II: 5, JJ: 6 },
    HH: { AA: 5, BB: 6, CC: 5, DD: 4, EE: 3, FF: 2, GG: 1, HH: 2, II: 6, JJ: 7 },
    II: { AA: 1, BB: 2, CC: 3, DD: 2, EE: 3, FF: 4, GG: 5, HH: 6, II: 2, JJ: 1 },
    JJ: { AA: 2, BB: 3, CC: 4, DD: 3, EE: 4, FF: 5, GG: 6, HH: 7, II: 1, JJ: 2 }
  }
  */
  return { valvesAndFlowRates, valvesHavingPositiveRate, valveBitRepr, shortestPathMatrix };
}

const isVisited = (bitReprA, bitReprB) => bitReprA & bitReprB;

function initializeFloydWarshallShortestPathMatrix(valves, valvesAndFlowRates) {
  return valves.reduce((acc, valveFrom) => {
    acc[valveFrom] = valves.reduce((acc, valveTo) => {
      acc[valveTo] = valvesAndFlowRates[valveFrom].to.includes(valveTo)
        ? 1
        : Number.POSITIVE_INFINITY
      return acc;
    }, {});
    return acc;
  }, {});
}

function getFloydWarshallShortestPathMatrix(valves, valvesAndFlowRates) {
  const shortestPathMatrix = initializeFloydWarshallShortestPathMatrix(valves, valvesAndFlowRates);

  // https://www.youtube.com/watch?v=4OQeCuLYj-4
  for (const mid of valves) {
    for (const start of valves) {
      for (const end of valves) {
        shortestPathMatrix[start][end] = Math.min(
          shortestPathMatrix[start][end],
          shortestPathMatrix[start][mid] + shortestPathMatrix[mid][end]
        )
      }
    }
  }

  return shortestPathMatrix;
}

function depthFirstSearch({ currentMinutesLeft, currentValve, currentRate, openedValvesBitRepr, maxRateMapBitRepr, originalData}) {

  // bitReprMaxRateMap = {}

  // If for the current valves opened, current rate is higher, update it
  maxRateMapBitRepr[openedValvesBitRepr] = Math.max(maxRateMapBitRepr[openedValvesBitRepr] || 0, currentRate);

  // maxRateMapBitRepr = { '0': 0 }

  for (const nextValve of originalData.valvesHavingPositiveRate) {
    const updatedMinutesLeft = currentMinutesLeft - (originalData.shortestPathMatrix[currentValve][nextValve] + 1);
    const updatedRate = currentRate + updatedMinutesLeft * originalData.valvesAndFlowRates[nextValve].rate;
    const updatedOpenedValvesBitRepr = openedValvesBitRepr | originalData.valveBitRepr[nextValve];

      // currentMinutesLeft = 30, currentValve = AA, valve = BB, shortestPathMatrix[AA][BB] = 1
      // valvesAndFlowRates[BB] = { rate: 13, to: [ 'CC', 'AA' ] }, valveBitRepr[BB] = 1 (000001), openedBitRepr = 000000
      // updatedMinutesLeft = 30 - 2 = 28;
      // updatedRate = 0 + 28 * 13
      // updatedOpenedValvesBitRepr = 000000 | 000001 = 000001

    if (updatedMinutesLeft > 0 && !isVisited(originalData.valveBitRepr[nextValve], openedValvesBitRepr)) {
      depthFirstSearch({
        currentMinutesLeft: updatedMinutesLeft,
        currentRate: updatedRate,
        currentValve: nextValve,
        openedValvesBitRepr: updatedOpenedValvesBitRepr,
        maxRateMapBitRepr,
        originalData,
      });
    }
  }

  return maxRateMapBitRepr;
}

function getMaxPressureReleased(input, part=1) {
  const originalData = prepareData(input);
  const maxRateMapBitRepr = depthFirstSearch({
    currentMinutesLeft: part === 1 ? 30 : 26,
    currentValve: "AA",
    currentRate: 0,
    openedValvesBitRepr: 0,
    maxRateMapBitRepr: {},
    originalData
  });

  if (part === 1) {
    return Math.max(...Object.values(maxRateMapBitRepr));
  }

  let res = Number.NEGATIVE_INFINITY;

  for (const human of Object.entries(maxRateMapBitRepr)) {
    for (const elephant of Object.entries(maxRateMapBitRepr)) {
      if (!(human[0] & elephant[0])) {
        res = Math.max(res, human[1] + elephant[1])
      }
    }
  }

  return res;
}

const sample = ["Valve AA has flow rate=0; tunnels lead to valves DD, II, BB", "Valve BB has flow rate=13; tunnels lead to valves CC, AA", "Valve CC has flow rate=2; tunnels lead to valves DD, BB", "Valve DD has flow rate=20; tunnels lead to valves CC, AA, EE", "Valve EE has flow rate=3; tunnels lead to valves FF, DD", "Valve FF has flow rate=0; tunnels lead to valves EE, GG", "Valve GG has flow rate=0; tunnels lead to valves FF, HH", "Valve HH has flow rate=22; tunnel leads to valve GG", "Valve II has flow rate=0; tunnels lead to valves AA, JJ", "Valve JJ has flow rate=21; tunnel leads to valve II"];
const input = ["Valve HM has flow rate=0; tunnels lead to valves LS, YS", "Valve IY has flow rate=15; tunnels lead to valves YI, MU, KN, QS, QM", "Valve VI has flow rate=22; tunnels lead to valves LE, SE, RB, JR", "Valve SE has flow rate=0; tunnels lead to valves VI, AZ", "Valve QU has flow rate=0; tunnels lead to valves YC, QK", "Valve RB has flow rate=0; tunnels lead to valves AN, VI", "Valve PU has flow rate=0; tunnels lead to valves JR, IM", "Valve OA has flow rate=0; tunnels lead to valves KZ, FR", "Valve AQ has flow rate=23; tunnels lead to valves FA, QM, GE", "Valve QS has flow rate=0; tunnels lead to valves IM, IY", "Valve HC has flow rate=24; tunnel leads to valve XH", "Valve QI has flow rate=0; tunnels lead to valves KQ, LS", "Valve FA has flow rate=0; tunnels lead to valves HA, AQ", "Valve BA has flow rate=0; tunnels lead to valves KZ, ME", "Valve DH has flow rate=0; tunnels lead to valves LT, HA", "Valve TE has flow rate=0; tunnels lead to valves AA, ZJ", "Valve AA has flow rate=0; tunnels lead to valves YS, XT, TE, GY, FS", "Valve YC has flow rate=9; tunnels lead to valves DV, XH, DJ, QU", "Valve KN has flow rate=0; tunnels lead to valves IY, AZ", "Valve GS has flow rate=0; tunnels lead to valves FS, KZ", "Valve DJ has flow rate=0; tunnels lead to valves YC, UV", "Valve GY has flow rate=0; tunnels lead to valves QK, AA", "Valve ZJ has flow rate=6; tunnels lead to valves RC, HS, UV, ME, TE", "Valve RC has flow rate=0; tunnels lead to valves BY, ZJ", "Valve QK has flow rate=10; tunnels lead to valves QU, XX, HS, RM, GY", "Valve AN has flow rate=0; tunnels lead to valves HA, RB", "Valve XT has flow rate=0; tunnels lead to valves AA, KQ", "Valve LT has flow rate=0; tunnels lead to valves IM, DH", "Valve YI has flow rate=0; tunnels lead to valves LE, IY", "Valve BK has flow rate=0; tunnels lead to valves LS, RM", "Valve LE has flow rate=0; tunnels lead to valves VI, YI", "Valve IM has flow rate=19; tunnels lead to valves PU, EC, QS, LT", "Valve SK has flow rate=0; tunnels lead to valves RF, AZ", "Valve RM has flow rate=0; tunnels lead to valves QK, BK", "Valve YM has flow rate=0; tunnels lead to valves LS, KZ", "Valve DV has flow rate=0; tunnels lead to valves YC, AI", "Valve QM has flow rate=0; tunnels lead to valves IY, AQ", "Valve KZ has flow rate=5; tunnels lead to valves BA, GS, YM, OA, XX", "Valve FS has flow rate=0; tunnels lead to valves GS, AA", "Valve UV has flow rate=0; tunnels lead to valves DJ, ZJ", "Valve AZ has flow rate=20; tunnels lead to valves SE, KN, SK, MS", "Valve BY has flow rate=0; tunnels lead to valves RC, LS", "Valve OY has flow rate=0; tunnels lead to valves KQ, EI", "Valve XX has flow rate=0; tunnels lead to valves KZ, QK", "Valve ME has flow rate=0; tunnels lead to valves BA, ZJ", "Valve YS has flow rate=0; tunnels lead to valves AA, HM", "Valve MS has flow rate=0; tunnels lead to valves AZ, HA", "Valve HS has flow rate=0; tunnels lead to valves QK, ZJ", "Valve LS has flow rate=3; tunnels lead to valves BK, HM, QI, BY, YM", "Valve KQ has flow rate=17; tunnels lead to valves OY, XT, QI", "Valve MU has flow rate=0; tunnels lead to valves IY, HA", "Valve EC has flow rate=0; tunnels lead to valves IM, GE", "Valve XH has flow rate=0; tunnels lead to valves HC, YC", "Valve JR has flow rate=0; tunnels lead to valves PU, VI", "Valve EI has flow rate=0; tunnels lead to valves OY, RF", "Valve AI has flow rate=25; tunnel leads to valve DV", "Valve GE has flow rate=0; tunnels lead to valves AQ, EC", "Valve RF has flow rate=18; tunnels lead to valves EI, FR, SK", "Valve FR has flow rate=0; tunnels lead to valves OA, RF", "Valve HA has flow rate=12; tunnels lead to valves AN, FA, MU, MS, DH"];

console.log('sample part 1', getMaxPressureReleased(sample, 1)); // 1651
console.log('input part 1', getMaxPressureReleased(input, 1)); // 2253
console.log('sample part 2', getMaxPressureReleased(sample, 2)); // 1707
console.log('input part 2', getMaxPressureReleased(input, 2)); // 2838
```
