### Part 1

```js
const input = ["$ cd /", "$ ls", "dir a", "14848514 b.txt", "8504156 c.dat", "dir d", "$ cd a", "$ ls", "dir e", "29116 f", "2557 g", "62596 h.lst", "$ cd e", "$ ls", "584 i", "$ cd ..", "$ cd ..", "$ cd d", "$ ls", "4060174 j", "8033020 d.log", "5626152 d.ext", "7214296 k"];

const route = [];
let currentDir = '/';
const hdd = { '/': 0 };
let total = 0;

input.forEach((lineString) => {
  const line = lineString.split(' ');
  if(line[0] === '$') {
    if(line[1] === 'cd') {
      if(line[2] === '..') {
        route.pop();
        currentDir = route[route.length-1] || '/';
      } else {
        currentDir = line[2];
        route.push(currentDir);
      }
    }
  } else if (line[0] === 'dir') {
    const dirName = line[1];
    hdd[route.join('.') + '.' + dirName] = 0;
  } else {
    const size = Number(line[0]);
    
    let parent = '';
    route.forEach((dir, i) => {
      parent = i === 0 ? dir : parent + '.' + dir;
      hdd[parent] += size;
    });
    total += size;
  }
});

console.log(hdd);

const finalValue = Object.values(hdd).reduce((acc, value) => {
  return value <= 100000 ? acc + value  acc;
}, 0);

console.log(finalValue);
console.log(total);
```
