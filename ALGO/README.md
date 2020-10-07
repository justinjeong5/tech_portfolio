<h1>ALGORITHM</h1>

- [Sorting](#sorting)
  - [Bubble Sort](#bubble-sort)
  - [Insertion Sort](#insertion-sort)
  - [Selection Sort](#selection-sort)
  - [Merge Sort](#merge-sort)
  - [Quick Sort](#quick-sort)
  - [Heap Sort](#heap-sort)
  - [Radix Sort](#radix-sort)
- [Union Find](#union-find)
    - [constructor](#constructor)
  - [makeSet](#makeset)
  - [findParent](#findparent)
  - [mergeSet](#mergeset)
  - [complexity](#complexity)
  - [implementation](#implementation)
    - [related problem - leetcode 399. Evaluate Division](#related-problem---leetcode-399-evaluate-division)
    - [related problem - 200. Number of Islands](#related-problem---200-number-of-islands)
- [backtracking](#backtracking)
  - [Depth First Search(DFS)](#depth-first-searchdfs)
  - [Breadth First Search(BFS)](#breadth-first-searchbfs)
  - [휴리스틱(heuristics)](#휴리스틱heuristics)
    - [related problem - Baekjoon 9663. N-Queen](#related-problem---baekjoon-9663-n-queen)
    - [related problem - baekjoon 2580. Sudoku](#related-problem---baekjoon-2580-sudoku)
- [TWO POINTERS](#two-pointers)
  - [time complexity](#time-complexity)
  - [one pointer with hashmap](#one-pointer-with-hashmap)
  - [sliding window](#sliding-window)

# Sorting

## Bubble Sort

![Bubble_sort_animation](https://user-images.githubusercontent.com/44011462/61772707-e75e7780-ae2d-11e9-8394-ec83d03749cc.gif)

**거품 정렬(Bubble Sort)** 은 두 인접한 원소를 검사하여 정렬하는 방법이다. 시간 복잡도가 *O(n^2)* 로 상당히 느리지만, 코드가 단순하기 때문에 자주 사용된다. 원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름이다.

    시간 복잡도: O(N^2)
    공간 복잡도: extra O(1)

```javascript
const bubbleSort = (array) => {
    let length = array.length;
    let i, j, temp;
    for (i = 0; i < length - 1; i++) { // 순차적으로 비교하기 위한 반복문
        for (j = 0; j < length - 1 - i; j++) { // 끝까지 돌았을 때 다시 처음부터 비교하기 위한 반복문
            if (array[j] <= array[j + 1]) continue // 두 수를 비교하여 앞 수가 뒷 수보다 작거나 같으면 통과
            array[j] = [array[j + 1], array[j + 1] = array[j]][0]; //swap
        }
    }
    return array;
}
```

## Insertion Sort

![Insertion_sort_animation](https://user-images.githubusercontent.com/44011462/61772654-c1d16e00-ae2d-11e9-9a0e-9aa5ea0807be.gif)


**삽입 정렬(insertion Sort)** 은 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘이다. k번째 반복 후의 결과 배열은, 앞쪽 k + 1 항목이 정렬된 상태이다.

![Insertionsort-before](https://user-images.githubusercontent.com/44011462/61772339-f85ab900-ae2c-11e9-91c9-693c443848db.png)
*Array prior to the insertion of x*

각 반복에서 정렬되지 않은 나머지 부분 중 첫 번째 항목은 제거되어 정확한 위치에 삽입된다. 그러므로 다음과 같은 결과가 된다. 이는 **inplace 알고리즘**의 특성을 갖게 한다.

![Insertionsort-after](https://user-images.githubusercontent.com/44011462/61772338-f85ab900-ae2c-11e9-856b-55f278903f3f.png)
*Array after the insertion of x*

배열이 길어질수록 효율이 떨어지지만, 구현이 간단하다는 장점이 있다. 선택 정렬이나 거품 정렬과 같은 같은 *O(n^2)* 알고리즘에 비교하여 빠르며, **안정(stable) 정렬**이다.

    시간 복잡도: O(N^2)
    공간 복잡도: extra O(1)


```javascript
const insertionSort = (array) => {
    let length = array.length;
    for (let i = 1; i < length; i++) {
        const key = array[i];
        let j = i - 1;
        while (j >= 0 && array[j] > key) {
            array[j + 1] = array[j];
            j = j - 1;
        }
        array[j + 1] = key;
    }
    return array;
};
```


## Selection Sort

![Selection-Sort-Animation](https://user-images.githubusercontent.com/44011462/61772691-d9105b80-ae2d-11e9-81af-ef19b2beed86.gif)


**선택 정렬(selection Sort)** 은 제자리 정렬 알고리즘의 하나로, 다음과 같은 순서로 이루어진다. 주어진 리스트 중에 최소값을 찾는다. 그 값을 맨 앞에 위치한 값과 교체한다. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.
비교하는 것이 상수 시간에 이루어진다는 가정 아래, n개의 주어진 리스트를 이와 같은 방법으로 정렬하는 데에는 *O(n^2)* 만큼의 시간이 걸린다. 선택 정렬은 알고리즘이 단순하며 사용할 수 있는 메모리가 제한적인 경우에 사용시 성능 상의 이점이 있습니다.

    시간 복잡도: O(n^2)
    공간 복잡도: extra O(1)

```javascript
const selectionSort = (array) => {
    let length = array.length;
    let minIndex, temp, i, j;
    for (i = 0; i < length - 1; i++) { // 처음부터 훑으면서
        minIndex = i;
        for (j = i + 1; j < length; j++) { // 최솟값의 위치를 찾음
            if (array[j] >= array[minIndex]) continue;
            minIndex = j;
        }
        array[i] = [array[minIndex], array[minIndex] = array[i]][0]; // 최솟값을 제일 앞으로 보냄
    }
    return array;
}
```

## Merge Sort

![220px-Merge-Sort-example-300px](https://user-images.githubusercontent.com/44011462/61772622-acf4da80-ae2d-11e9-8798-d75ab2c0ffd7.gif)

**병합 정렬(merge Sort)** 은 *O(n log n)* 비교 기반 정렬 알고리즘이다. 일반적인 방법으로 구현했을 때 이 정렬은 안정 정렬에 속하며, **분할 정복(divide and conquer) 알고리즘**의 하나이다. 존 폰 노이만이 1945년에 개발했다.

    시간 복잡도: O(n log n)
    공간 복잡도: extra O(n)

```javascript
const mergeSort = (array) => {
    if (array.length < 2) 
        return array; // 원소가 하나일 때는 그대로 내보냅니다.
    const pivot = Math.floor(array.length / 2); // 대략 반으로 쪼개는 코드
    const left = array.slice(0, pivot); // 쪼갠 왼쪽
    const right = array.slice(pivot, array.length); // 쪼갠 오른쪽
    return merge(mergeSort(left), mergeSort(right)); // 재귀적으로 쪼개고 합칩니다.
}

const merge = (left, right) => {
    const result = [];
    while (left.length && right.length) {
        if (left[0] <= right[0]) { // 두 배열의 첫 원소를 비교하여
            result.push(left.shift()); // 더 작은 수를 결과에 넣어줍니다.
        } else {
            result.push(right.shift()); // 오른쪽도 마찬가지
        }
    }
    while (left.length) result.push(left.shift()); // 어느 한 배열이 더 많이 남았다면 나머지를 다 넣어줍니다.
    while (right.length) result.push(right.shift()); // 오른쪽도 마찬가지

    return result;
};

```
## Quick Sort

![220px-Sorting_quicksort_anim](https://user-images.githubusercontent.com/44011462/61772579-977fb080-ae2d-11e9-92fe-44de99119834.gif)

**퀵 정렬(quicksort)** 은 찰스 앤터니 리처드 호어가 개발한 정렬 알고리즘이다. 다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬에 속한다. 퀵 정렬은 n개의 데이터를 정렬할 때, 최악의 경우에는 *O(n^2)* 번의 비교를 수행하고, 평균적으로 O(n log n)번의 비교를 수행한다.

**메모리 참조가 지역화** 되어 있기 때문에 CPU 캐시의 히트율이 높아지기 때문에, 퀵 정렬의 내부 루프는 대부분의 컴퓨터 아키텍처에서 효율적으로 작동하도록 설계되어있다. 따라서 대부분의 실질적인 데이터를 정렬할 때 제곱 시간이 걸릴 확률이 거의 없도록 알고리즘을 설계하는 것이 가능하다. 때문에 일반적인 경우 퀵 정렬은 다른 *O(n log n)* 알고리즘에 비해 훨씬 빠르게 동작한다. 그리고 퀵 정렬은 정렬을 위해 *O(1)* 만큼의 추가 공간을 필요로한다. 또한 퀵 정렬은 불안정 정렬에 속한다.

    시간 복잡도: O(n^2)
    공간 복잡도: extra O(1)

```javascript
const partition = (array, left, right, pivotIndex) => { // 정렬하는 부분
    let temp;
    let pivot = array[pivotIndex];
    while (left <= right) { // 왼쪽, 오른쪽 수를 규칙과 비교해 다음 수로 넘어갑니다.
        while (array[left] < pivot)
            left++;
        while (array[right] > pivot)
            right--;
        if (left <= right) { // 왼쪽이 기준보다 크고, 오른쪽이 기준보다 작으면
            temp = array[left];
            array[left] = array[right];
            array[right] = temp; // 서로 바꿔줍니다.
            left++;
            right--;
        }
    }
    array[left] = [array[pivotIndex], array[pivotIndex] = array[left]][0]; // 마지막으로 기준과 만난 수를 바꿔줍니다. 기준의 위치는 이제 i입니다.
    return left;
};

const quickSort = (array, left, right) => { // 재귀하는 부분
    if (!left) left = 0;
    if (!right) right = array.length - 1;
    let pivotIndex = right; // 배열 가장 오른쪽의 수를 기준으로 뽑습니다.
    pivotIndex = partition(array, left, right - 1, pivotIndex); // right - 1을 하는 이유는 현재 원소를 제외하고 정렬하기 위함입니다.
    if (left < pivotIndex - 1)
        quickSort(array, left, pivotIndex - 1); // 기준 왼쪽 부분 재귀
    if (pivotIndex + 1 < right)
        quickSort(array, pivotIndex + 1, right); // 기준 오른쪽 부분 재귀
    return array;
};
```

## Heap Sort

**힙 정렬(Heapsort)** 이란 최대 힙 트리나 최소 힙 트리를 구성해 정렬을 하는 방법으로서, 내림차순 정렬을 위해서는 최대 힙을 구성하고 오름차순 정렬을 위해서는 최소 힙을 구성하면 된다. 

![힙소트2](https://user-images.githubusercontent.com/44011462/88031020-fa0da880-cb76-11ea-8a34-e3bd07e71a22.gif)

    시간 복잡도: O(n log n)
    공간 복잡도: extra O(n)

```javascript
let arrLen;

const heapRoot = (array, i) => {
    const left = 2 * i + 1;
    const right = 2 * i + 2;
    let max = i;

    if (left < arrLen && array[left] > array[max]) {
        max = left;
    }

    if (right < arrLen && array[right] > array[max]) {
        max = right;
    }

    if (max != i) {
        array[i] = [array[max], array[max] = array[i]][0];
        heapRoot(array, max);
    }
}

const heapSort = (array) => {
    arrLen = array.length;

    for (let i = Math.floor(arrLen / 2); i >= 0; i--) {
        heapRoot(array, i);
    }

    for (let i = array.length - 1; i > 0; i--) {
        array[0] = [array[i], array[i] = array[0]][0];
        arrLen--;

        heapRoot(array, 0);
    }

    return array;
}

```

## Radix Sort

**기수 정렬(radix Sort)** 는 대단히 빠르고 자리수를 비교해서 정렬하는 방식입니다. 단점이라면 자리수가 없는 것들은 정렬할 수 없다는 것입니다. 예를 들면 부동소수점같은 경우가 있습니다. 하지만 문자열과 정수는 거의 다 정렬할 수 있습니다. 

시간 복잡도는 *O(dn)* 이고. d는 가장 큰 데이터의 자리수입니다. 빅 oh 표기법에 따르면 O(dn)은 O(n)에 속하기 때문에 빠릅니다. 가장 작은 자리수부터 비교하는 *LSD*방법입니다. 가장 큰 자리수부터 비교하는 방법은 *MSD*라고 불립니다.

![radix-Sort-image](https://user-images.githubusercontent.com/44011462/88031327-6688a780-cb77-11ea-8c59-df8ed4cffeac.png)


    시간 복잡도: O(dn)
    공간 복잡도: extra O(d + n)

```javascript
const counter = [[]];
const radixLSD = (array, d) => {
    let mod = 10;
    for (let i = 0; i < d; i++, mod *= 10) { // mod는 현재 정렬 중인 자리수를 나타내는 것으로 10부터 해서 100, 1000, ...으로 커집니다.
        for (let j = 0; j < array.length; j++) {
            const bucket = parseInt(array[j] % mod); // 같은 그룹으로 묶일 나머지를 나타내는 부분입니다.
            if (counter[bucket] == null ) {
                counter[bucket] = [];
            }
            counter[bucket].push(array[j]); // 나머지 별로 묶어줍니다.
            // console.log('bucket', bucket, counter[bucket]);
        }
        // console.log(counter.slice(0));
        let pos = 0;
        for (let j = 0; j < counter.length; j++) { // counter에 저장한 묶음들(나머지 순서로 정렬됨)을 실제 배열에 반영해줍니다.
            let value = null ;
            if (counter[j] != null ) {
                while ((value = counter[j].shift()) != null ) {
                    array[pos++] = value;
                }
            }
        }
        // console.log(array);
    }
    return array;
}
```
# Union Find

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
## makeSet
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

## findParent
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

## mergeSet
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

## complexity

| Algorithm	| Average | Worst case |
|---:|---|---|
| Space | O(n) | O(n) |
| Search | O(α(n)) | O(α(n)) |
| Merge | O(α(n)) | O(α(n)) |


## implementation

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

### related problem - leetcode 399. Evaluate Division
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

### related problem - 200. Number of Islands

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

# backtracking

그래프에서 모든 정점(*Vertices*)을 방문하는 일을 그래프 순회(*Graph Traversal*)이라고 합니다. 그 중에서 원하는 특정 경로를 찾는 일을 그래프 탐색(*Graph Search*)라고 합니다. 생활에서 마주하는 문제를 그래프를 이용할 수 있도록 모델링 할 수 있기 때문에 그래프 탐색은 매우 유용한 알고리즘입니다.

1. 미로 찾기
<img src="https://user-images.githubusercontent.com/44011462/90377612-2d2e5380-e0b3-11ea-8821-0f4762a6b3ac.png" width="300px"><img src="https://user-images.githubusercontent.com/44011462/90377631-34edf800-e0b3-11ea-86f7-8d05dba48312.png" width="300px">

2. 최단 경로 찾기
<img src="https://user-images.githubusercontent.com/44011462/90378009-cb221e00-e0b3-11ea-8ad1-59e0dcd54e5f.png" width="300px">

3. 네비게이션
<img src="https://user-images.githubusercontent.com/44011462/90378371-3cfa6780-e0b4-11ea-87cd-878aa20882a5.png" width="300px">

4. TicTacToe
<img src="https://user-images.githubusercontent.com/44011462/90378495-66b38e80-e0b4-11ea-9966-b269c3d58588.png" width="300px">

5. 바둑(go)
<img src="https://user-images.githubusercontent.com/44011462/90378547-7337e700-e0b4-11ea-8f58-3a681d5beec0.png" width="300px">

위와 같은 그래프 탐색에는 대표적으로 1. **Depth First Search**, 2. **Breadth First Search**가 사용된다. DFS와 BFS는 각각 깊이와 너비를 우선적으로 탐색하는 방식을 의미한다. 위 두가지 방식은 아래의 GIF를 보면 직관적으로 이해할 수 있다. 

<img src="https://user-images.githubusercontent.com/44011462/61773130-d9f5bd00-ae2e-11e9-8bfc-41d13a269061.gif" width="600px">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>  
    https://www.uniquesoftwaredev.com/tag/dfs/
</details>

DFS와 BFS는 탐색을 우선시 하는 방식에 차이가 있고 그에 따른 특징이 다르게 나타납니다.
## Depth First Search(DFS)
1. 스택(Stack)을 이용 - 재귀(Recursion)
2. 더이상 확인할 길이 없을때까지 들어갔다가 나오며 경로를 순회한다.
3. 그래프에 싸이클이 있거나 깊이가 무한히 깊으면 적절하지 못하다.
4. Memory의 Heap 영역에 메모리를 적게 사용한다.

***N*** * ***N*** 크기의 2차원 배열에서 최단 경로를 찾는다고 할때,
시간복잡도 : ***O(2^N)***
공간복잡도 : ***O(N)***

```c++
그래프의 깊이 우선 탐색(DFS))
vector<vector<int>>adj;    // 그래프의 인접 리스트표현 
vector<bool> visited;   // 각 정점을 방문했는지 여부를나타낸다. 
 
void dfs(int here){     // 깊이 우선 탐색을 구현한다.
    cout << "DFS visits " << here << endl; 
    visited [here] = true; 
    for(int i = 0; i < adj[here].size(); ++i) {     // 모든 인접 정점을 순회하면서 
        int there = adj[here][i];   // 아직 방문한적 없다면 방문한다. 
        if( !visited[there]) 
            dfs(there); 
    } 
    // 더 이상 방문할 정점이 없으니, 재귀 호출을 종료하고 이전 정점으로 돌아간다. 
} 

void dfsAll() {     // 모든 정점을 방문한다. 
    visited = vector<bool>(adj.size(), false);   // visited를 모두 false로 초기화한다. 
    for(int i = 0; i < adj.size(); ++i) {    // 모든 정점을 순회하면서, 아직 방문한적 없으면 방문한다.
        if (!visited [i]) 
            dfs(i); 
    }
}
```

## Breadth First Search(BFS)
1. 큐(Queue)를 이용
2. 현재 위치에서 갈 수 있는 루트를 우선적으로 순회한다.
3. 최단 경로를 찾을 수 있는 방식이다.
4. Memory에 공간이 많이 필요하다.

***N*** * ***N*** 크기의 2차원 배열에서 최단 경로를 찾는다고 할때,
시간복잡도 : ***O(2^N)***
공간복잡도 : ***O(2^N)***

```c++
그래프의 너비 우선 탐색(BFS)
vector <vector<int>> adj; 	// 그래프의 인접 리스트표현 
vector<int> bfs(int start) { 	//start에서 시작해 그래프를 너비 우선 탐색하고 각 정점의 방문 순서를 반환한다. 
	vector<bool> discovered(adj.size(), false);		//각 정점의 방문 여부 
	queue<int> q; 	 	// 방문할정점 목록올 유지하는 큐 
	vector<int> order;		//정점의 방문순서 
	discovered[start] = true; 
	q.push(start);
	while(!q.empty()) {
		int here = q. front(); 
		q.pop();
		order.push_back(here);		//here를 방문한다.
		for(int i = 0; i < adj[here].size(); i++) {	// 모든 인접한 정점을 검사한다.
			int there = adj[here][i];
			if(!discovered[there]) { //처음 보는 정점이면 방문 목록에 집어 넣는다. 
				q.push(there); 
				discovered[there] = true; 
			}
		}
	}
	return order;
}
```

## 휴리스틱(heuristics)

백트래킹(**backtracking**)은 어느 지점에서 문제가 제약조건을 만족하지 못하는 부분을 제거해 나가면서 해답에 점차 가까워지는 문제 해결 전략이다. DFS와 유사하게 흐름이 진행되지만 **DFS와 Backtracking을 완전히 다르게** 볼 수 있는 것은 바로 **부분 문제(subproblem)을 제거**해 나간다는 **휴리스틱(heuristics)** 에 있다. backtracking에서의 휴리스틱은 **정답이 될 수 없는 것을 제외시키는 정도**로 사용된다. 

    어떤 문제가 있을 때 그 문제를 푸는 방법이 아직 없거나 현실적으로 불가능할 때, 혹은 
    문제를 풀기 위한 정보가 완전히 주어지지 않을 때, 
    확립된 절차에 따라 답을 구할 수 있을 정도로 문제가 명확하게 정의되지 않았을 때
    휴리스틱이 사용 가능하다.

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>  
    위키백과 휴리스틱 이론<br>
    https://ko.wikipedia.org/wiki/%ED%9C%B4%EB%A6%AC%EC%8A%A4%ED%8B%B1_%EC%9D%B4%EB%A1%A0<br>
</details>


스도쿠(sudoku)문제를 푼다고 가정해보자. 빈공간에 숫자를 하나씩 채워나가다가 이미 정답이 될 수 없는 숫자배열이 나온다면 그 숫자를 지우고(backtrack) 다음숫자를 배치하면서 답을 찾아나간다.
모든 가능한 경우를 전부다 확인해보는 방식보다 더 빠르다. backtrack하면서 부분 문제(Subset)을 제거했기 때문이다. 


### related problem - Baekjoon 9663. [N-Queen](https://www.acmicpc.net/problem/9663)

```c++
/*
 *   2020.8.18
 *   Baekjoon 9663. N-Queen
 */
#include <iostream>
#include <vector>
#include <utility>

using namespace std;

int input_size;
int answer = 0;
vector<vector<int>> board;
vector<vector<int>> queen;

void resize_input() {
    board.resize(input_size, vector<int>(input_size));
}

void print_answer() {
    cout << answer;
}

bool validator(int &row, int &col) {
    for (vector<int> &it : queen) {
        if (it[1] == col) return false;
        if (it[0] - it[1] == row - col) return false;
        if (it[0] + it[1] == row + col) return false;
    }
    return true;
}

void backtracking(int &depth) {
    if (depth + 1 == input_size) {
        answer++;
        return;
    }
    int next = depth + 1;
    for (int index = 0; index < input_size; ++index) {
        if (!validator(next, index)) continue;
        queen.push_back({next, index});
        backtracking(next);
        queen.pop_back();
    }
}

void solution() {
    resize_input();

    int depth = -1;
    backtracking(depth);

    print_answer();
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    cin >> input_size;
    solution();

    return 0;
}
```

### related problem - baekjoon 2580. [Sudoku](https://www.acmicpc.net/problem/2580)

```c++
/*
 * 2020.8.18
 * Baekjoon 2580_스도쿠.cpp
 */
#include <iostream>
#include <vector>
#include <utility>

using namespace std;

vector<vector<int>> board;

void print_answer() {
    for (int row = 0; row < 9; ++row) {
        for (int col = 0; col < 9; ++col) {
            cout << board[row][col] << " ";
        }
        cout << "\n";
    }
}

bool check_row(int &col, int &target) {
    for (int row = 0; row < 9; ++row) {
        if (board[row][col] == target) return false;
    }
    return true;
}

bool check_col(int &row, int &target) {
    for (int col = 0; col < 9; ++col) {
        if (board[row][col] == target) return false;
    }
    return true;
}

bool check_box(int &row, int &col, int &target) {
    const int target_y = (row / 3) * 3;
    const int target_x = (col / 3) * 3;

    for (int dy = 0; dy < 3; ++dy) {
        for (int dx = 0; dx < 3; ++dx) {
            if (board[target_y + dy][target_x + dx] == target) return false;
        }
    }
    return true;
}

bool validator(int &pos, int &target) {
    int row = pos / 9;
    int col = pos % 9;
    return check_box(row, col, target) && check_row(col, target) && check_col(row, target);
}

void backtracking(int &pos) {
    if (pos == 80) {
        print_answer();
        // exit(0);
        return;
    }

    int next = pos + 1;
    int row = next / 9;
    int col = next % 9;

    if (board[row][col] != 0) {
        backtracking(next);
        return;
    }

    for (int val = 1; val < 10; ++val) {
        if (!validator(next, val)) continue;
        board[row][col] = val;
        backtracking(next);
        board[row][col] = 0;
    }
}

void solution() {
    int start = -1;
    backtracking(start);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    board.resize(9, vector<int>(9));

    for (int row = 0; row < 9; ++row) {
        for (int col = 0; col < 9; ++col) {
            cin >> board[row][col];
        }
    }

    solution();

    return 0;
}

```

# TWO POINTERS

**Two pointers**는 기본적으로 동시에 **2곳을 중점적으로 관찰**하는 방식을 말한다. 각각의 포인터가 동시에 같은 방향으로 진행하거나 서로 마주오는 방향으로 진행할 수 있다. two pointers의 개념이 베어있는 가장 대표적인 예시가 sorting algorithm이라고 할 수 있다. 아래의 코드는 javascript로 작성된 selection sort의 알고리즘이다. 
```javascript
const selectionSort = () => {
    var indexMin;
    for (var left = 0; left < numSiz; ++left) {
        indexMin = left;
        for (var right = left + 1; right < numSiz; ++right) {
            if (nums[right] >= nums[indexMin]) continue;
            indexMin = right;
        }
        [nums[left], nums[indexMin]] = [nums[indexMin], nums[left]];
    }
}
```
selection sorting의 특징은 두 개의 loop가 중첩(nested)되어 있고 **각각의 loop가 가진 index 정보가 각각 하나의 pointer의 역할**을 한다. 아래의 그림을 보면 **두개의 포인터**가 움직이면서 swap을 통해 정렬을 진행하고 있다.

![Selection-Sort-Animation](https://user-images.githubusercontent.com/44011462/61772691-d9105b80-ae2d-11e9-81af-ef19b2beed86.gif)

## time complexity

**two pointers**는 일반적으로 <img src="https://user-images.githubusercontent.com/44011462/95317267-e3eacb00-08cf-11eb-8147-62b4e34b6d14.png" height=23px>의 시간복잡도를 갖는다.  *기본적으로 two pointers로 해결가능한 문제는 2개의 loop를 이용하여 <img src="https://user-images.githubusercontent.com/44011462/95316988-7e96da00-08cf-11eb-9f5f-d758d70c8f3b.png" height=23px>의 시간복잡도를 갖는 solution을 만들 수 있다*. 다시말해 nested-loop의 solution이 존재하는 문제중에서 **특정 조건이 만족된다면 two pointers를 이용**한 간단한 solution을 만들 수 있다.  
따라서 naive solution으로 이중 반복문의 형태를 갖는다면 two pointer가 가능하지 확인해야 한다. 위에서 예시로 들은 sort는 two pointers로 풀수 있는 조건을 갖추지 못한 문제의 유형이다. 왼쪽 pointer에 해당하는 변수 left가 다시 돌아오는 구조를 갖기 떄문이다. 
참고로 **sorting**은 ***O(n log(n))의 사간복잡도*** 가 **optimal**으로 증명된 유형의 문제이다. 문제에 대한 해법이 최적화된 형태를 갖는다는 말에는 많은 의미가 내포되어있다. 이 부분은 나중에 따로 떼어 살펴보도록 하자.

## one pointer with hashmap
혹은 **하나의 pointer에 hashmap**과 같은 다른 자료구조를 이용해서 two pointer의 효과를 갖을 수 있다.  
하나의 pointer는 중첩되지 않은 loop에, 또다른 pointer는 hashmap에 두는 유형이다. 엄밀히 구분하자면 이는 <img src="https://user-images.githubusercontent.com/44011462/95317267-e3eacb00-08cf-11eb-8147-62b4e34b6d14.png" height=23px>의 시간복잡도를 갖는 one pointer algorithm과 <img src="https://user-images.githubusercontent.com/44011462/95318680-d33b5480-08d1-11eb-9af6-57ae1e025a95.png" height=23px>의 시간복잡도를 갖는 hashmap을 이용한 방식이지만 2곳을 중점적으로 관찰한다는 점에서 two pointers의 한가지 종류로 생각했다.  문제를 통해서 설명해보자.  
[문제보기](https://leetcode.com/problems/two-sum/)

    Given an array of integers nums and an integer target, 
    return the two numbers such that they add up to target.

    You may assume that each input would have exactly one solution, 
    and you may not use the same element twice.

    You can return the answer in any order.

        Example 1:

        Input: nums = [2,7,11,15], target = 9
        Output: [2,7]
        Output: Because 2 + 7 == 9, we return [2, 9].

위 문제는 nested-loop로 <img src="https://user-images.githubusercontent.com/44011462/95316988-7e96da00-08cf-11eb-9f5f-d758d70c8f3b.png" height=23px>의 시간복잡도를 갖는 solution을 간단하게 떠올릴 수 있는 형태의 문제이다. 따라서 **two pointers**가 적용이 가능할지 살펴볼 수 있다. two pointers는 pointer의 진행방향이 빈번하게 바뀌는 경우에는 사용할 수 없다. 위 문제의 경우에는 **two pointers방식을 적용하기에는 무리**가 있는데, left에 해당하는 포인터의 진행방향은 일정하지만 **right에 해당하는 pointer의 진행방향이 일정하지 않기 때문**이다. 그럼에도 two pointer로 분류할 수 있는 이유는 a + b = c라는 수식에서 2개의 미지수가 정해지면 마지막 하나는 저절로 값이 정해지는 특징이 있기 떄문이다. 

```javascript
const twoSum = (nums, target) => {
    const map = [];
    for (const num of nums) {
        let complement = target - num;
        if (map[complement] !== undefined) {
            return [complement, num];
        }
        map[complement] = map[num] = true;
    }
    return [-1, -1]
}
```


## sliding window

**two pointer**가 같은 방향으로 향하는 유형의 문제에서 유명한 방식으로 **sliding window algorithm**이 있다. 앞서 말한 것처럼 sliding window로 해결가능한 문제는 nested-loop의 방식으로도 해결이 가능하다. 하지만 수행시간에 있어서 상당한 차이를 보이기 떄문에 문제의 조건이 **sliding window를 쓸수 있는 유형**이라면 <img src="https://user-images.githubusercontent.com/44011462/95317267-e3eacb00-08cf-11eb-8147-62b4e34b6d14.png" height=23px>의 시간복잡도를 갖는 two pointers의 한 종류인 sliding window algorithm을 사용해야 한다.
sliding window에도 여러가지 유형이 있지만 가장 기본적인 형태를 갖는 문제를 통해 설명해보자. [문제보기](https://leetcode.com/problems/minimum-window-substring/)

    Given a string S and a string T, 
    find the minimum window in S which will contain all the characters in T 

        Example 1:

        input: S = "ABAACBAB", T = "ABC"
        Outpur: "ACB"


아래의 그림을 보면 일종의 크기가 늘어나고 줄어드는 창문이 움직이는 것처럼 보인다. 원래 sliding window는 *network* 분야에서 처음 고안되었다. 송신자와 수신자간의 packet의 흐름을 제어하는 [congestion control](https://github.com/justinjeong5/tech_portfolio/tree/master/NW#congestion-control)을 구현하기 위한 방법으로 사용된다. **실시간성이 매우 중요한 네트워크**에서 naive한 방식을 채택하는 것이 논리에 맞지 않을 뿐더러 네트워크에서는 절차를 시작할때 모든 정보를 알지 못해도 **입력을 차례로 받아들이면서 문제를 해결하는 online algorithm**이어야 한다는 점이 있고 sliding window는 이러한 특징을 매우 잘 반영하고 있다. 

<img src="https://user-images.githubusercontent.com/44011462/95321263-c3257400-08d5-11eb-8b99-c1e25658b55a.png" width=400px>   
<img src="https://user-images.githubusercontent.com/44011462/95321272-c7ea2800-08d5-11eb-8ede-6dbd9a246647.png" width=400px>  
<img src="https://user-images.githubusercontent.com/44011462/95321288-cf113600-08d5-11eb-92bf-b25d91590ac9.png" width=400px>  
<img src="https://user-images.githubusercontent.com/44011462/95321308-dafcf800-08d5-11eb-9afa-737fda3cd8d0.png" width=400px>  
<img src="https://user-images.githubusercontent.com/44011462/95321327-e2240600-08d5-11eb-9e70-c8ea8f78f5dd.png" width=400px>  
<img src="https://user-images.githubusercontent.com/44011462/95321526-1d263980-08d6-11eb-8b91-83524cda11d8.png" width=400px>  

코드를 통해 two pointers의 특징을 살펴보자. 아래의 코드를 보면 **start와 end라는 변수가 two pointers의 역할**을 하고 있다. 위 문제는 단순히 two pointers만으로는 풀수 없기 때문에 hashmap와 count변수를 도입하여 처리하고 있다. 풀이법에 따라서 queue를 사용하여 풀수도 있지만 모두가 two pointers의 개념을 사용한다는 점에는 변함이 없다. 

```javascript

var minWindow = function (s, t) {

    let remains = 0;
    let answer = "";
    const um = [];

    const preprocess = () => {
        for (let it of t) {
            if (um[it] === undefined) {
                um[it] = 0;
                remains++;
            }
            um[it]++;
        }
    }

    const solution = () => {
        let start = 0, end = 0;
        let len = Number.MAX_VALUE;
        let answer_start = 0;

        while (end < s.length) {
            const endChar = s[end++];
            if (um[endChar] !== undefined) {
                if (--um[endChar] === 0) remains--;
            }
            while (remains === 0) {
                if (len > end - start) {
                    len = end - start;
                    answer_start = start;
                }
                const startChar = s[start++];
                if (um[startChar] !== undefined) {
                    if (++um[startChar] > 0) remains++;
                }
            }
        }
        if (len === Number.MAX_VALUE) return;
        answer = s.substr(answer_start, len);
    }

    preprocess();
    solution();
    return answer;
};

```

