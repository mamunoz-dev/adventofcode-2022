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
  const valvesHavingPositiveRate = valves.filter(valve => Boolean(valvesAndFlowRates[valve].rate));
  const valveBitRepr =
    valvesHavingPositiveRate.reduce((acc, valve, i) => Object.assign(acc, { [valve]: 1 << i }), {});
  const shortestPathMatrix = getFloydWarshallShortestPathMatrix(valves, valvesAndFlowRates);

  return [ valvesAndFlowRates, valvesHavingPositiveRate, valveBitRepr, shortestPathMatrix ];
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

function depthFirstSearch({ minutesLeft, currentValve, openedBitRepr, rate, bitReprMaxRateMap, valvesHavingPositiveRate, shortestPathMatrix, valveBitRepr, valvesAndFlowRates}) {
  bitReprMaxRateMap[openedBitRepr] = Math.max(bitReprMaxRateMap[openedBitRepr] || 0, rate);

  for (const valve of valvesHavingPositiveRate) {
    const newMinutesLeft = minutesLeft - (shortestPathMatrix[currentValve][valve] + 1);
    const newRate = rate + newMinutesLeft * valvesAndFlowRates[valve].rate;
    const newOpenedBitRepr = openedBitRepr | valveBitRepr[valve];

    if (newMinutesLeft <= 0 || isVisited(valveBitRepr[valve], openedBitRepr))
      continue;

    depthFirstSearch({
      minutesLeft: newMinutesLeft,
      currentValve: valve,
      openedBitRepr: newOpenedBitRepr,
      rate: newRate,
      bitReprMaxRateMap,
      valvesHavingPositiveRate,
      shortestPathMatrix,
      valveBitRepr,
      valvesAndFlowRates
    });
  }

  return bitReprMaxRateMap;
}

function getMaxPressureReleased(input, part=1) {
  const [ valvesAndFlowRates, valvesHavingPositiveRate, valveBitRepr, shortestPathMatrix ] = prepareData(input);
  const bitReprMaxRateMap = depthFirstSearch({
    minutesLeft: part === 1 ? 30 : 26,
    currentValve: "AA",
    openedBitRepr: 0,
    rate: 0,
    bitReprMaxRateMap: {},
    valvesHavingPositiveRate,
    shortestPathMatrix,
    valveBitRepr,
    valvesAndFlowRates
  });

  if (part === 1) {
    return Math.max(...Object.values(bitReprMaxRateMap));
  }

  let res = Number.NEGATIVE_INFINITY;

  for (const human of Object.entries(bitReprMaxRateMap)) {
    for (const elephant of Object.entries(bitReprMaxRateMap)) {
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
console.log('input part 1', getMaxPressureReleased(input, 2)); // 2838
```
