## Union Find

**Union find**는 미시건 대학의 [Bernard Galler](#https://en.wikipedia.org/wiki/Bernard_Galler) 에 의해 1964년에 처음 고안된 disjoint set의 data structure를 이용하는 algorithm이다. Union find는 **disjoint-set**, **merge-find** 라고도 불린다. 
Union find는 *multiway tree*의 형태를 다루며 원소간의 **partition**을 매우 빠르고 효율적으로 다루는 알고리즘이다. 

### constructor
disjoint set의 구성요소는 문제를 모델링한 결과에 따라 차이가 있지만 대부분 parent, size, rank를 갖는다. parent[]는 임의의 원소의 부모가 누구인지 담아두는 공간이다. size[]는 임의의 원소가 속한 그룹의 크기를 의미한다. rank[]는 임의의 원소가 속한 그룹이 root로부터 얼마나 떨어져 있는지 담아두는 공간이다.
```javascript
class UnionFind{
    constructor(){
        this._parent = [];
        this._setSize = [];
        this._rant = [];
    }
}
```
### makeSet
위에서 선언된 공간을 union find에서 활용하기 위해서는 초기값이 필요하다. 이 연산을 수행하는 함수를 makeSet이라고 부르며 다음과 같이 구현된다. 초기값으로 임의의 원소 newNode의 parent값은 자기 자신으로 한다. 자신이 root이므로 rank는 0이고 자기 자신으로만 구성된 set이므로 size는 1이다.

```javascript
class UnionFind{
    constructor(){
        ...
    }
    makeSet(newNode){
        if(this._parent[newNode] === undefined){
            this._parent[newNode] = newNode;
            this._setSize[newNode] = 1;
            this._rant[newNode] = 0;
        }
    }
}
```

### findParent
makeSet으로 disjoint set에 포함된 임의의 원소를 찾는 연산이다. 이를 구현하는 방법에는 크게 3가지가 있고 **Path compression**, **Path halving**, **Path splitting**의 방식이 있다.

```javascript
Path compression
const findParent = (node) => {
    if (this._parent[node] === node) return node;
    return this._parent[node] = findParent(this._parent[node]);
}
```

```javascript
Path halving
const findParent = (node) => {
    while(this._parent[node] !== node){
        this._parent[node] = this._parent[this._parent[node]];
        node = this._parent[node];
    }
    return node;
}
```

이중 Path compression방식은 다른 방식에 비해 성능이 훨씬 좋다. 임의의 원소의 parent를 탐색과 동시에 경로를 압축하게되어 재탐색시에 O(1)의 성능을 보장한다.

<img width="760" alt="Screen Shot 2020-07-29 at 4 35 52 PM" src="https://user-images.githubusercontent.com/44011462/88771099-b5f35700-d1b9-11ea-8e5e-abf6482f3fe3.png">
<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html  <br>
find 연산 최적화<br> 
</details>

### mergeSet
두 서로다른 set을 하나로 합치는 방식은 크게 2가지가 있다. size, 또는 rank를 이용하는 방식이다. 대부분 size가 큰것을 root로 삼거나 rank가 작은 것을 root로 삼는다. 

```javascript
size가 더 큰 것을 root로 삼는 방법

const mergeSet = (node1, node2) => {
    node1 = findParent(node1);
    node2 = findParent(node2);
    if(node1 === node2) return;
    if(this._setSize[node1] < this._setSize[node2]){
        [node1, node2] = [node2, node1];
    }
    this._parent[node2] = node1;
    this._setSize[node1] += this._setSize[node2];
    this._setSize[node2] = 1;
}
```

```javascript
rank가 더 작은 것을 root로 삼는 방법

const mergeSet = (node1, node2) => {
    node1 = findParent(node1);
    node2 = findParent(node2);
    if(node1 === node2) return;
    if(this._rank[node1] < this._rank[node2]){
        [node1, node2] = [node2, node1];
    }
    this._parent[node2] = node1;
    if(this._rank[node1] === this._rank[node2]){
        this._rank[node1]++;
    }
}
```

### complexity

| Algorithm	| Average | Worst case |
|---:|---|---|
| Space | O(n) | O(n) |
| Search | O(α(n)) | O(α(n)) |
| Merge | O(α(n)) | O(α(n)) |


### implementation

```javascript

class UnionFind{
   constructor(){
        this._parent = [];
        this._setSize = [];
    }
    makeSet(newNode){
        if(this._parent[newNode] === undefined){
            this._parent[newNode] = newNode;
            this._setSize[newNode] = 1;
        }
    }
    //Path compression
    findParent(node){
        if (this._parent[node] === node) return node;
        return this._parent[node] = findParent(this._parent[node]);
    }
    //merging criteria: smaller setSize
    mergeSet(node1, node2){
        node1 = findParent(node1);
        node2 = findParent(node2);
        if(node1 === node2) return;
        if(this._setSize[node1] < this._setSize[node2]){
            [node1, node2] = [node2, node1];
        }
        this._parent[node2] = node1;
        this._setSize[node1] += this._setSize[node2];
        this._setSize[node2] = 1;
    }
}
```

**reference**

[wikipedia](#https://en.wikipedia.org/wiki/Disjoint-set_data_structure#Representation)   
[gmlwjd9405 (github)](#https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html)  

#### related problem - leetcode 399. Evaluate Division
<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
leetcode 399. Evaluate Division<br>
https://leetcode.com/problems/evaluate-division/<br>
</details>  

Equations are given in the format ***A / B = k***, where ***A*** and ***B*** are variables represented as strings, and ***k*** is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return ***-1.0***.

Example:

    Given a / b = 2.0, b / c = 3.0.
    queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .
    return [6.0, 0.5, -1.0, 1.0, -1.0 ].

The input is:

    vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries,
    where equations.size() == values.size(), and the values are positive. 
    This represents the equations. 
    Return vector<double>.

According to the example above:

    equations = [ ["a", "b"], ["b", "c"] ],
    values = [2.0, 3.0],
    queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ],
    expected = [6.00000,0.50000,-1.00000,1.00000,-1.00000].
 
The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.

```javascript
/**
 * 2020.7.29
 * LeetCode 399_Evaluate_Division.js
 * 
 * https://leetcode.com/problems/evaluate-division/
 * calculate division of two unknowns by referring to condition
 * 
 * data structure and algorithm: disjoint set and union find
 * 
 * when N is the number of elements of equation
 * time complexity: O(α(N))
 * space complexity: O(N)
 */

/**
 * @param {string[][]} equations
 * @param {number[]} values
 * @param {string[][]} queries
 * @return {number[]}
 */
function calcEquation(equations, values, queries) {
    const parent = [];
    const children = [];
    const weight = [];

    const mergeSet = (v1, v2, w) => {
        let r1 = findParent(v1);
        let r2 = findParent(v2);

        if (r1 === r2) return;
        if (children[r1].length > children[r2].length) {
            [r1, r2, v1, v2, w] = [r2, r1, v2, v1, 1 / w];
        }

        w *= weight[v2] / weight[v1];
        for (let c of children[r1]) {
            children[r2].push(c);
            parent[c] = r2;
            weight[c] *= w;
        }
        delete children[r1];
    }

    const findParent = (v) => {
        if (parent[v] === v) return v;
        return parent[v] = findParent(parent[v]);
    }

    const registerParent = (v) => {
        if (parent[v] === undefined) {
            parent[v] = v;
            children[v] = [v];
            weight[v] = 1;
        }
    }

    for (let i = 0; i < values.length; i++) {
        registerParent(equations[i][0]);
        registerParent(equations[i][1]);
        mergeSet(equations[i][0], equations[i][1], values[i]);
    }

    const res = [];
    for (let [c1, c2] of queries) {
        if (parent[c1] === undefined || parent[c2] === undefined || parent[c1] !== parent[c2]) {
            res.push(-1);
            continue;
        }
        res.push(weight[c1] / weight[c2]);
    }

    return res;
}
```

#### related problem - 200. Number of Islands

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
leetcode 200. Number of Islands<br>
https://leetcode.com/problems/number-of-islands/<br>
</details>  

Given a 2d grid map of ***'1'*** s (land) and ***'0'*** s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:

Input:

    grid = [
        ["1","1","1","1","0"],
        ["1","1","0","1","0"],
        ["1","1","0","0","0"],
        ["0","0","0","0","0"]
    ]
Output: 

    1

Example 2:

Input: 

    grid = [
        ["1","1","0","0","0"],
        ["1","1","0","0","0"],
        ["0","0","1","0","0"],
        ["0","0","0","1","1"]
    ]
Output: 

    3


```javascript
/**
 * 2020.7.29
 * LeetCode 200_NumberOfIslands.js
 * 
 * https://leetcode.com/problems/number-of-islands/
 * find the number of island on given map
 * 
 * data structure and algorithm: disjoint set and union find
 * 
 * when N is the number of cell on map
 * time complexity: O(α(N))
 * space complexity: O(N)
 */

class UnionFind {
    constructor() {
        this._parent = [];
        this._numberOfIsland = 0;
    }
    makeSet(node) {
        if (this._parent[node] === undefined) {
            this._parent[node] = node;
            this._numberOfIsland++;
        }
    }
    findParent(node) {
        if (this._parent[node] === node) return node;
        return this._parent[node] = this.findParent(this._parent[node]);
    }
    mergeSet(node1, node2) {
        node1 = this.findParent(node1);
        node2 = this.findParent(node2);
        if (node1 === node2) return;

        if (node1 > node2) {
            [node1, node2] = [node2, node1];
        }
        this._parent[node2] = node1;
        this._numberOfIsland--;
    }
}

const dir = [{ row: 1, col: 0 }, { row: 0, col: 1 }, { row: -1, col: 0 }, { row: 0, col: -1 }];

/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    const ROW_SIZ = grid.length;
    if (ROW_SIZ === 0) return 0;
    const COL_SIZ = grid[0].length;

    const checkBoundary = (pos) => {
        return pos.row >= 0 && pos.row < ROW_SIZ && pos.col >= 0 && pos.col < COL_SIZ;
    }

    const isWater = (pos) => {
        return grid[pos.row][pos.col] === '0';
    }

    const uf = new UnionFind();

    for (let row = 0; row < ROW_SIZ; ++row) {
        for (let col = 0; col < COL_SIZ; ++col) {
            if (isWater({ row, col })) continue;
            const CUR_NODE = row * COL_SIZ + col;
            uf.makeSet(CUR_NODE);
            for (const pos of dir) {
                const next = { row: row + pos.row, col: col + pos.col };
                if (!checkBoundary(next) || isWater(next)) continue;
                const NEXT_NODE = (row + pos.row) * COL_SIZ + (col + pos.col);
                uf.makeSet(NEXT_NODE);
                uf.mergeSet(CUR_NODE, NEXT_NODE);
            }
        }
    }
    return uf._numberOfIsland;
};
```