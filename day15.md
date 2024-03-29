# Day 15: Beacon Exclusion Zone

Codepen: https://codepen.io/mamunoz-dev/pen/GRBOQxN?editors=0012

Added progress log because the execution takes a while:
```js
function showProgress(y, limit) {
  if (limit % y === 0) {
    const progress = y/limit*100;
    if (progress >= 1) {
      console.log(y/limit*100, '%');
    }
  }
}

function parse(input) {
  return input.map(line => {
    const [, sx, sy, bx, by] = line.match(
      /^Sensor at x=(-?\d+), y=(-?\d+): closest beacon is at x=(-?\d+), y=(-?\d+)$/,
    );
    return { sx: +sx, sy: +sy, bx: +bx, by: +by };
  });
}

function getSensorAndBeaconPositions(input) {
  const sensors = [];
  const beacons = [];

  input.forEach(line => {
    let [sensor, beacon] = line.split(':');
    sensor = sensor.trim().replace('Sensor at ', '').trim();
    beacon = beacon.trim().replace('closest beacon is at ', '').trim();

    sensor = sensor.replace('x=','').replace('y=','').trim().split(',').map(Number)
    beacon = beacon.replace('x=','').replace('y=','').trim().split(',').map(Number)

    sensors.push({ x: sensor[0], y: sensor[1]});
    beacons.push({ x: beacon[0], y: beacon[1]});
  })

  return [sensors, beacons];
}

function getManhattanDistance(a, b) {
  return Math.abs(a.x - b.x) + Math.abs(a.y - b.y);
}

function getEmptyPositions(sensors, beacons, y) {
  const emptyPositions = new Set();

  sensors.forEach((sensor, i) => {
    showProgress(i, sensors.length);

    const beacon = beacons[i];

    const manhattanDistance = getManhattanDistance(sensor, beacon);
    const x = manhattanDistance - Math.abs(sensor.y - y);
    const startRange = sensor.x - x;
    const endRange = sensor.x + x;

    for(let x=startRange; x<=endRange; x++) {
      const beaconPresent = x === beacon.x && y === beacon.y;
      if(!beaconPresent) {
        emptyPositions.add(JSON.stringify([x,y]));
      }
    }
  });

  return emptyPositions.size;
}

function part1(input, y) {
  const [sensors, beacons] = getSensorAndBeaconPositions(input);
  return getEmptyPositions(sensors, beacons, y);
}

function part2(input, limit){
  const manhattanDistanceMap = new Map();

  const [sensors, beacons] = getSensorAndBeaconPositions(input);

  sensors.forEach((sensor, i) => {
    const beacon = beacons[i];
    const manhattanDistance = getManhattanDistance(sensor, beacon);
    manhattanDistanceMap.set(sensor, manhattanDistance);
  });

  let visible = false;
  for (let y=0; y<limit; y++) {
    showProgress(y, limit);

    for (let x=0; x<limit; x++) {
      visible = false;

      for (const sensor of manhattanDistanceMap.keys()) {
        const manhattanDistance = manhattanDistanceMap.get(sensor);
        const currentManhattanDistance = getManhattanDistance(sensor, {x, y});

        if (currentManhattanDistance <= manhattanDistance) {
          x = sensor.x + (manhattanDistance - Math.abs(sensor.y - y))
          visible = true;
          break;
        }
      }

      if (!visible) {
        return x * 4000000 + y;
      }
    }
  }
}

const sample = ["Sensor at x=2, y=18: closest beacon is at x=-2, y=15", "Sensor at x=9, y=16: closest beacon is at x=10, y=16", "Sensor at x=13, y=2: closest beacon is at x=15, y=3", "Sensor at x=12, y=14: closest beacon is at x=10, y=16", "Sensor at x=10, y=20: closest beacon is at x=10, y=16", "Sensor at x=14, y=17: closest beacon is at x=10, y=16", "Sensor at x=8, y=7: closest beacon is at x=2, y=10", "Sensor at x=2, y=0: closest beacon is at x=2, y=10", "Sensor at x=0, y=11: closest beacon is at x=2, y=10", "Sensor at x=20, y=14: closest beacon is at x=25, y=17", "Sensor at x=17, y=20: closest beacon is at x=21, y=22", "Sensor at x=16, y=7: closest beacon is at x=15, y=3", "Sensor at x=14, y=3: closest beacon is at x=15, y=3", "Sensor at x=20, y=1: closest beacon is at x=15, y=3"];
const input = ["Sensor at x=220580, y=684270: closest beacon is at x=436611, y=263737", "Sensor at x=3329538, y=3016377: closest beacon is at x=3355914, y=2862466", "Sensor at x=2605308, y=2023938: closest beacon is at x=2197530, y=2271330", "Sensor at x=1810202, y=3423309: closest beacon is at x=1829362, y=3182862", "Sensor at x=480296, y=3999646: closest beacon is at x=1694700, y=4178942", "Sensor at x=46556, y=1283362: closest beacon is at x=-91140, y=1441882", "Sensor at x=3741660, y=3959257: closest beacon is at x=3537901, y=3368697", "Sensor at x=3399994, y=700264: closest beacon is at x=3748004, y=2000000", "Sensor at x=1531981, y=3801761: closest beacon is at x=1694700, y=4178942", "Sensor at x=193367, y=2712458: closest beacon is at x=-91140, y=1441882", "Sensor at x=3199067, y=2194575: closest beacon is at x=3748004, y=2000000", "Sensor at x=1878117, y=2578817: closest beacon is at x=2197530, y=2271330", "Sensor at x=2439089, y=3168242: closest beacon is at x=1829362, y=3182862", "Sensor at x=273443, y=171076: closest beacon is at x=436611, y=263737", "Sensor at x=3680413, y=2477027: closest beacon is at x=3748004, y=2000000", "Sensor at x=3620241, y=2904998: closest beacon is at x=3355914, y=2862466", "Sensor at x=1728351, y=2895399: closest beacon is at x=1829362, y=3182862", "Sensor at x=1894207, y=1168355: closest beacon is at x=2197530, y=2271330", "Sensor at x=856867, y=3271314: closest beacon is at x=1829362, y=3182862", "Sensor at x=3056788, y=2626224: closest beacon is at x=3355914, y=2862466", "Sensor at x=3598024, y=3322247: closest beacon is at x=3537901, y=3368697", "Sensor at x=1662543, y=3128823: closest beacon is at x=1829362, y=3182862", "Sensor at x=3992558, y=1933059: closest beacon is at x=3748004, y=2000000", "Sensor at x=1844282, y=2994285: closest beacon is at x=1829362, y=3182862", "Sensor at x=3604375, y=3668021: closest beacon is at x=3537901, y=3368697", "Sensor at x=2569893, y=3911832: closest beacon is at x=1694700, y=4178942", "Sensor at x=117970, y=37503: closest beacon is at x=436611, y=263737", "Sensor at x=3951385, y=3125577: closest beacon is at x=3537901, y=3368697", "Sensor at x=2482373, y=2648092: closest beacon is at x=2197530, y=2271330", "Sensor at x=915040, y=1835970: closest beacon is at x=-91140, y=1441882", "Sensor at x=3047883, y=3301452: closest beacon is at x=3537901, y=3368697", "Sensor at x=117432, y=1503889: closest beacon is at x=-91140, y=1441882", "Sensor at x=1136011, y=261705: closest beacon is at x=436611, y=263737", "Sensor at x=2343111, y=66183: closest beacon is at x=2081841, y=-807749", "Sensor at x=608229, y=955721: closest beacon is at x=436611, y=263737", "Sensor at x=1189379, y=3999750: closest beacon is at x=1694700, y=4178942", "Sensor at x=766640, y=26597: closest beacon is at x=436611, y=263737", "Sensor at x=3891093, y=2110588: closest beacon is at x=3748004, y=2000000"];

console.log('part 1 sample', part1(sample, 10)); // 26
console.log('part 1 input', part1(input, 2000000)); // 4886370
console.log('part 2 sample', part2(sample, 20)); // 56000011
console.log('part 2 input', part2(input, 4000000)); // 11374534948438
```
