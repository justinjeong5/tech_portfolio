**컴공 주요 5과목에 대한 주요 개념 정리**

**<목차>**
- [FRONT-END](#front-end)
- [DESIGN PATTERN](#design-pattern)
  - [Facade pattern](#facade-pattern)
  - [Adaptor pattern](#adaptor-pattern)
- [ALGORITHM](#algorithm)
  - [정렬 (sorting)](#정렬-sorting)
    - [거품 정렬(bubble sort)](#거품-정렬bubble-sort)
    - [삽입 정렬(insertion sort)](#삽입-정렬insertion-sort)
    - [선택 정렬(selection sort)](#선택-정렬selection-sort)
    - [병합 정렬(merge sort)](#병합-정렬merge-sort)
    - [퀵 정렬(quick sort)](#퀵-정렬quick-sort)
    - [힙 정렬(heap sort)](#힙-정렬heap-sort)
    - [기수 정렬(radix sort)](#기수-정렬radix-sort)
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
- [DATABASE](#database)
  - [Transaction](#transaction)
    - [ACID](#acid)
      - [atomicity](#atomicity)
    - [transaction의 장단점](#transaction의-장단점)
    - [COMMIT과 ROLLBACK](#commit과-rollback)
- [OPERATING SYSTEM](#operating-system)
- [NETWORK](#network)
  - [UDP (User Datagram Protocol), TCP (Transmission Control Protocol)](#udp-user-datagram-protocol-tcp-transmission-control-protocol)
    - [다중화(Multiplexing)와 역다중화(Demultiplexing)](#다중화multiplexing와-역다중화demultiplexing)
      - [비연결형 다중화](#비연결형-다중화)
      - [연결형 다중화](#연결형-다중화)
    - [web-server와 TCP](#web-server와-tcp)
    - [UDP](#udp)
      - [UDP가 TCP보다 좋은점](#udp가-tcp보다-좋은점)
      - [UDP segment 구조](#udp-segment-구조)
      - [UDP check-sum](#udp-check-sum)
    - [신뢰적인 데이터 전달(reliable data transfer)의 원리](#신뢰적인-데이터-전달reliable-data-transfer의-원리)
    - [TCP](#tcp)
      - [TCP segment 구조](#tcp-segment-구조)
      - [흐름 제어 (Flow Control)](#흐름-제어-flow-control)
      - [TCP Three-way Handshake](#tcp-three-way-handshake)
      - [혼잡 제어 (Congestion Control)](#혼잡-제어-congestion-control)
        - [슬로 스타트 (Slow Start)](#슬로-스타트-slow-start)
        - [혼잡 회피 (Congestion Avoidance)](#혼잡-회피-congestion-avoidance)
        - [빠른 회복 (Fast Recovery)](#빠른-회복-fast-recovery)
- [SECURITY](#security)
  - [steganography](#steganography)
    - [상용 스테가노그래피(steganopraphy)](#상용-스테가노그래피steganopraphy)
    - [steganography 구현](#steganography-구현)
      - [개요](#개요)
      - [사용방법](#사용방법)
      - [실행결과](#실행결과)
      - [구현원리](#구현원리)
      - [소스코드](#소스코드)
  - [AI SUMMIT 2020, SEOUL](#ai-summit-2020-seoul)
    - [EPP(Endpoint Protection Platform)](#eppendpoint-protection-platform)
    - [EDR(Endpoint Detection and Response)](#edrendpoint-detection-and-response)
    - [AI for Malware Detection](#ai-for-malware-detection)
    - [AXI(Explainable AI) for Security](#axiexplainable-ai-for-security)
    - [Security for AI](#security-for-ai)
      - [Evasion Attack(Adversarial Example)](#evasion-attackadversarial-example)
      - [Data Poisoning](#data-poisoning)
      - [AI Model Stealing](#ai-model-stealing)
  - [SQL Injection](#sql-injection)
    - [blind sql injection](#blind-sql-injection)
    - [sql injection 방어](#sql-injection-방어)
- [COMPUTER ARCHITECTURE](#computer-architecture)


# FRONT-END

# DESIGN PATTERN

## Facade pattern

오늘도 카페에 가서 아이스 아메리카노를 주문합니다. 점원에게 아이스 아메리카노 한잔을 주문하고 카드로 결제를 하면 주문은 간단하게 끝납니다. 커피의 재고가 있는지, 테이크아웃 용기는 충분한지는 중요한 일이 아닙니다. 점원에게 재고를 파악하고 커피를 만들수 있는 방법이 있기 때문에 고객의 입장에서 주문은 간단합니다. 이와 같은 패턴이 소프트웨어어도 적용됩니다. 복잡하고 내부적인 내용은 감추고 간단한 겉모습만 제공하는 design pattern을 말합니다. 

<img src="https://user-images.githubusercontent.com/44011462/90099244-f84d9400-dd74-11ea-8e00-a9d9b20924a0.png" width="400px">  

*Facade는 건물의 외관을 뜻하는 프랑스어로, 복잡한 내부가 아닌, 세련된 외관을 중요시한 design pattern을 Facade pattern이라고 한다.*


```javascript
// order.js

class Order{
    constructor(){
        this.coffeeOrderArr = [];
        this.coffeeStock = 10;
        this.container = 5;
    }

    validCoffeeStock(){
        return this.coffeeStock > 0;
    }

    vaildContainer(){
        return this.container > 0;
    }

    makeCoffee(){
        if(validCoffeeStock() && vaildContainer()){
            this.coffeeOrderArr.shift();
            this.coffeeStock--;
            this.container--;
            return true;
        }
        return false;
    }

    orderCoffee(item){
        this.coffeeOrderArr.push(item);
        return makeCoffee();
    }   
}
```

고객의 입장에서는 커피의 재고가 있는지, 테이크아웃 용기는 충분한지 고민할 필요가 없습니다. 그저 커피를 달라고 요청하면 복잡하고 세부적인 절차가 수행됩니다. 
```javascript
// main.js
import Order from './order.js'

const order = new Order();
const res = order.orderCoffee();
if(res){
    console.log("주문하신 커피가 준비되었습니다."); // 주문하신 커피가 준비되었습니다.
} else {
    console.log("재고가 부족합니다.");
}
```

이처럼 프로그램을 잘 모르는 사용자에게 최소한의 API만 노출하는 것을 **Facade pattern**이라고 합니다. 

## Adaptor pattern

실생활에서 adaptor는 쉽게 볼 수 있다. 대표적으로 USB2.0의 micro 5pin 충전기를 USB3.0의 c-type로 바꿔주는 스마트폰 adaptor가 있고 해외 여행을 가려면 가장 먼저 준비하는 110v - 250v **돼지코**가 있다. 이 adaptor를 사용하는 이유는 간편하기 때문이다. 휴대폰이 바뀔때마다 충전기를 바꿔야하는 불편함은 어느정도 해소가 가능하지만, *해외여행을 갈따마다 110v용 전자기기를 구입하는 것은 억지스럽다*.

<img src="https://user-images.githubusercontent.com/44011462/90095919-f7b0ff80-dd6c-11ea-9735-a45e9e7f4120.png" width="200px">
<img src="https://user-images.githubusercontent.com/44011462/90095893-e4059900-dd6c-11ea-8a36-d1e24fd7d3ad.png" width="200px">


소프트웨어에서도 동일한 일이 발생한다. 여러가지 이유로 인해 기존의 구조를 새로운 구조로 변경하려고 할때마다 기존의 것을 이용하지 못하고 새로 만들어야 한다면 *앞서 살펴본 억지스러운 일*이 될 수 있다. 소프트웨어에서 **돼지코**의 역할을 하는 design pattern이 **Adaptor pattern**이다. 

흔히 사용하는 adaptor 예제인 **Printer class**를 살표보자. 기본적인 생성자, 글자를 입력하는 함수, 출력하는 함수가 있는 class이다.
```javascript
// Printer.js
class Printer{
    constuctor(){
        this.textArr = [];
    }

    pushText(text){
        this.textArr.push(text);
    }

    print(){
        return this.textArr.join(' ');
    }
}
```

위에서 Printer를 만들었고 다음과 같은 코드를 이용하여 문자를 console에 출력 할 수 있다.
```javascript
import Printer from './Printer.js'

const printer = new Printer();
printer.pushText("hello");
printer.pushText("Adapter");
printer.pushText("design");
printer.pushText("pattern");

const result = printer.print();
console.log(result);    // hello Adpater design pattern
```

위와 같은 소프트웨어를 사용하다가 인스타그램이 널리 이용되게 되면서 '#'(hash tag)를 붙이는 Printer로 바꾸어야 한다고 가정하자. 아래를 새롭게 만들어야하는 **HashTagPrinter**의 Interface(명세)이다.
```javascript
// HashTagPrinter.js 
class HashTagPrinter { 
    constructor() { 
        this.textArr = []; 
    } 

    pushText(text) { 
        this.textArr.push(text); 
    }

    printWithHashTag() { 
        // print -> printWithHashTag로 변경 
        return this.textArr.map(text => `#${text}`).join(' '); 
    }
}
```

위에서 전달받은 새로운 HashTagPrinter를 이용하여 기존에 가지고 있던 main.js에 **그대로 붙여서 사용**하려고 한다. *마치 갤럭시 S10e에 USB 2.0 micro 5pin 충전기를 사용하려는 것처럼*
```javascript
import HashTagPrinter from './HashTagPrinter.js'

const printer = new HashTagPrinter();
printer.pushText("hello");
printer.pushText("Adapter");
printer.pushText("design");
printer.pushText("pattern");

const result = printer.print();     // interface가 들어맞지 않아서 에러가 발생한다.
console.log(result);   
```

이 상황에서 USB 2.0 micro 5pin 충전기를 USB 3.0 c-type 충전기로 바꾸기에는 *시간과 비용적인 측면에서 부담*스럽다. 이때 필요한 **Adaptor**를 코드로 구현해보자. 다시 말해서 *Printer에서 print()가 사용되었을때, HashTagPrinter의 printWithHashTag()를 동작*하도록 만들면 된다.

```javascript
// HashTagAdapter.js
class HashTagAdapter{
    constructer(hashTagPrinter){
        this.printer = hashTagPrinter;
    }

    pushText(item){
        this.printer.pushText(text);
    }

    print(){
        return this.printer.printWithHash();
    }
}
```

아래와 같은 코드가 앞서 만든 Adapter를 이용하여 기존의 print()를 가지고 원하는 결과를 출력할 수 있다. client 입장에서는 아무런 변화없이 기존의 서비스를 이용하는 것과 같은 효과를 낼 수 있다. 코드가 바뀔 떄 마다 client도 변화한다면 이는 매우 억지스러운 일이다.

```javascript
// main.js
import HashTagPrinter from './HashTagPrinter.js'
import HashTagAdapter from './HashTagAdapter.js'

const printer = new HashTagAdapter(new HashTagPrinter());
printer.pushText("hello");
printer.pushText("Adapter");
printer.pushText("design");
printer.pushText("pattern");

const result = printer.print();     
console.log(result);   // #hello #Adapter #design #pattern
```


https://dev-momo.tistory.com/entry/Adapter-Pattern-%EC%96%B4%EB%8C%91%ED%84%B0-%ED%8C%A8%ED%84%B4  
https://www.zerocho.com/category/JavaScript/post/57babe9f5abe0c17006fe230  


# ALGORITHM

## 정렬 (sorting)

### 거품 정렬(bubble sort)

![Bubble_sort_animation](https://user-images.githubusercontent.com/44011462/61772707-e75e7780-ae2d-11e9-8394-ec83d03749cc.gif)

**거품 정렬(Bubble sort)** 은 두 인접한 원소를 검사하여 정렬하는 방법이다. 시간 복잡도가 *O(n^2)* 로 상당히 느리지만, 코드가 단순하기 때문에 자주 사용된다. 원소의 이동이 거품이 수면으로 올라오는 듯한 모습을 보이기 때문에 지어진 이름이다.

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

### 삽입 정렬(insertion sort)

![Insertion_sort_animation](https://user-images.githubusercontent.com/44011462/61772654-c1d16e00-ae2d-11e9-9a0e-9aa5ea0807be.gif)


**삽입 정렬(insertion sort)** 은 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘이다. k번째 반복 후의 결과 배열은, 앞쪽 k + 1 항목이 정렬된 상태이다.

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


### 선택 정렬(selection sort)

![Selection-Sort-Animation](https://user-images.githubusercontent.com/44011462/61772691-d9105b80-ae2d-11e9-81af-ef19b2beed86.gif)


**선택 정렬(selection sort)** 은 제자리 정렬 알고리즘의 하나로, 다음과 같은 순서로 이루어진다. 주어진 리스트 중에 최소값을 찾는다. 그 값을 맨 앞에 위치한 값과 교체한다. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.
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

### 병합 정렬(merge sort)

![220px-Merge-sort-example-300px](https://user-images.githubusercontent.com/44011462/61772622-acf4da80-ae2d-11e9-8798-d75ab2c0ffd7.gif)

**병합 정렬(merge sort)** 은 *O(n log n)* 비교 기반 정렬 알고리즘이다. 일반적인 방법으로 구현했을 때 이 정렬은 안정 정렬에 속하며, **분할 정복(divide and conquer) 알고리즘**의 하나이다. 존 폰 노이만이 1945년에 개발했다.

    시간 복잡도: O(n log n)
    공간 복잡도: extra O(1)

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
### 퀵 정렬(quick sort)

![220px-Sorting_quicksort_anim](https://user-images.githubusercontent.com/44011462/61772579-977fb080-ae2d-11e9-92fe-44de99119834.gif)

**퀵 정렬(quicksort)** 은 찰스 앤터니 리처드 호어가 개발한 정렬 알고리즘이다. 다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬에 속한다. 퀵 정렬은 n개의 데이터를 정렬할 때, 최악의 경우에는 *O(n^2)* 번의 비교를 수행하고, 평균적으로 O(n log n)번의 비교를 수행한다.

**메모리 참조가 지역화** 되어 있기 때문에 CPU 캐시의 히트율이 높아지기 때문에, 퀵 정렬의 내부 루프는 대부분의 컴퓨터 아키텍처에서 효율적으로 작동하도록 설계되어있다. 따라서 대부분의 실질적인 데이터를 정렬할 때 제곱 시간이 걸릴 확률이 거의 없도록 알고리즘을 설계하는 것이 가능하다. 때문에 일반적인 경우 퀵 정렬은 다른 *O(n log n)* 알고리즘에 비해 훨씬 빠르게 동작한다. 그리고 퀵 정렬은 정렬을 위해 *O(log n)* 만큼의 추가 공간을 필요로한다. 또한 퀵 정렬은 불안정 정렬에 속한다.

    시간 복잡도: O(n log n)
    공간 복잡도: extra O(log n)

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

### 힙 정렬(heap sort)

**힙 정렬(Heapsort)** 이란 최대 힙 트리나 최소 힙 트리를 구성해 정렬을 하는 방법으로서, 내림차순 정렬을 위해서는 최대 힙을 구성하고 오름차순 정렬을 위해서는 최소 힙을 구성하면 된다. 

![힙소트2](https://user-images.githubusercontent.com/44011462/88031020-fa0da880-cb76-11ea-8a34-e3bd07e71a22.gif)

    시간 복잡도: O(n log n)
    공간 복잡도: extra O(1)

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

### 기수 정렬(radix sort)

**기수 정렬(radix sort)** 는 대단히 빠르고 자리수를 비교해서 정렬하는 방식입니다. 단점이라면 자리수가 없는 것들은 정렬할 수 없다는 것입니다. 예를 들면 부동소수점같은 경우가 있습니다. 하지만 문자열과 정수는 거의 다 정렬할 수 있습니다. 

시간 복잡도는 *O(dn)* 이고. d는 가장 큰 데이터의 자리수입니다. 빅 oh 표기법에 따르면 O(dn)은 O(n)에 속하기 때문에 빠릅니다. 가장 작은 자리수부터 비교하는 *LSD*방법입니다. 가장 큰 자리수부터 비교하는 방법은 *MSD*라고 불립니다.

![radix-sort-image](https://user-images.githubusercontent.com/44011462/88031327-6688a780-cb77-11ea-8c59-df8ed4cffeac.png)


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

## backtracking

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
### Depth First Search(DFS)
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

### Breadth First Search(BFS)
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

### 휴리스틱(heuristics)

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


#### related problem - Baekjoon 9663. [N-Queen](https://www.acmicpc.net/problem/9663)

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

#### related problem - baekjoon 2580. [Sudoku](https://www.acmicpc.net/problem/2580)

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



# DATABASE

## Transaction

transaction은 한 단위로 간주되어야 하는 작업의 단위를 말한다. 주로 은행거래의 예시를 들어 설명하는데, 만약 고객1이 고객2에게 100만원을 전송하려는 시도를 한다고 가정하자. 고객1의 계좌에서 잔액이 100만원이 줄어들면서 고객2의 계좌에서는 잔액이 100만원이 올라야한다. 어떤 특정한 이유에서 고객1의 잔액을 줄었지만 고객2의 잔고가 늘지 않는다면 이는 큰 문제가 된다. 이는 마치 고객1에서 빠져나간 돈이 아무에게도 전해지지 않고 사라지는 것과 같은 상황이다. 위 예시보다 합리적이라면 실제로 100만원이 이체되거나 혹은 아무일도 일어나지 않아야 한다. 정보를 생성하고 참조하며 수정하거나 지우는 역할을 수행하는 Database에서도 이와 동일한 이슈가 있다. 

### ACID

#### atomicity
atomicity는 여러가지 논리적으로 의미있는 단위의 일을 하나로 묶어서 반드시 한번에 일어나거나 전혀 일어나지 않아야 하는 일의 속성을 말한다. 어떤 행위에 atomicity가 있다고 하면 더이상 쪼개어질수 없다는 것과 같은 의미이다. 이는 database뿐 아니라 operating system의 critical section에서도 적용되는 개념이다. atomicity가 보장되려면 다음과 같은 일이 하나의 작업으로 보장되어야 한다.
1. 고객1이 10만원을 출금하려고 한다.
2. database에서 잔고를 확인하고 10만원을 뺸다
3. 고객1에게 10만원을 지급한다.

위 모든 과정이 마치 하나의 일로 발생해야하고, 중간에 어떤 특정한 이유로 모두 마무리 지을 수 없다면 아주 없던일로 해야한다. 

 **Consistency**
data는 constraint와 rule이 있어서 의미가 발생한다. 통장계좌는 항상 0와 같거나 큰 정수라고 정해져 있어야만 있지도 않은 돈을 주는 일을 막을 수 있다. 따라서 transaction간에 해당 data가 rule과 constraint를 지키도록 보장하는 것을 말한다. 만약 고객1이 있지도 않은 금액을 출금하려고 한다면 해당 trasaction은 중단되어야햔다.  
1. 고객1의 잔고는 100만원이다.
2. 고객1은 200만원을 출금하려고 시도한다.
3. 2번은 1번에대한 consistancy를 위반한다.
4. 해당 transaction은 중단된다


 **Isolation**
isolation은 transaction의 수행이 다른 transaction의 영향을 받지 않아야 한다는 것을 의미한다. 만약 고객1과 고객2가 동일한 계좌에서 금액을 출금하려고 한다고 하자. 계좌에는 100만원이 들어있고 고객1과 고객2는 각각 10만원, 25만원을 출금하려는 상황이다. 다음과 같은 일이 순서대로 일어났다고 가정하자. 
1. 고객1이 10만원을 출금한다.
2. 계좌에서 10만원이 줄어들고 이 내용이 database에 반영되려고 한다.
3. 2의 내용이 반영되기 전에 고객2가 25만원을 출금하려 시도했다.
4. 고객2가 25만월 지급받았다.
5. 고객2가 출금한 25만큼 database에서 잔액이 줄어들었다.
6. 2에서 반영된 내용이 이제서야 database에 반영된다.
7. 고객1과 고객2가 모두 출금하였고 은행 잔고는 90만원이다.

고객1이 10만원을 출금하는 transaction이 isolation을 보장받지 못한다면 고객2가 25만원을 출금하더라도 잔액이 여전히 90만원일 수 있다. 이런 일이 일어나지 않도록 보장하는 속성이 ioslation이다. 

 **Durability - Persistency**
persistency로도 불리는 durability는 문제없이 수행된 transaction에 대해서는 영원하게 자료의 내용이 보장되어야 한다는 의미이다. 특정 시스템의 문제가 발생하더라도 자료의 내용이 유지되어야 한다. 따라서 영구적으로 저장이 가능한 매체에 기록되어 저장되어야한다. 소프트웨어적으로도 이를 보장해야한다.

### transaction의 장단점
**Advantages**
1. 동시에 여러 사용자가 computer resource를 공유할 수 있다
2. computing resource가 이미 점유중이라면 job processing의 순서를 조정하여 유연하게 사용할 수 있다.
3. 사람의 개입 없이도 작업이 가능하여 computer resource를 효율적으로 사용할 수 있다.  

**Disadvantages**
1. 비용이 상대적으로 비싸다
2. 하드웨어와 소프트웨어가 호환성이 매우 떨어진다

_references_

[What is a database transaction? (stackoverflow)](#https://stackoverflow.com/questions/974596/what-is-a-database-transaction)  
[What is a Transaction? (microsoft)](#https://docs.microsoft.com/ko-kr/windows/win32/ktm/what-is-a-transaction?redirectedfrom=MSDN)  
[Transaction processing (wikipidea)](#https://en.wikipedia.org/wiki/Transaction_processing#ACID_criteria)  

### COMMIT과 ROLLBACK

Transaction의 구분은 commit을 이용하여 가능하다. commit은 모든 일의 과정이 정상적으로 진행되었으므로 모든 사항을 database에 반영하라는 의미이다. 이와 대응되는 개념은 rollback으로 data처리 과정에서 하나의 transaction을 마무리 할 수 없으므로, transaction을 되돌려 없던 일로 만들겠다는 의미이다. 이 commit과 rollback은 transaction이 가능하게 해주는 명령어이다. 비교하자면 문서작업을 할때 ctrl+z를 눌러서 일을 되돌리는 것을 rollback에, 문서작업이 어느정도 진행되면 ctrl+s를 눌러 저장하는 것을 commit에 비유할 수 있다. 다만 둘간에 매우 큰 차이가 있다면 ctrl+s를 누르면 더이상 ctrl+z를 사용하지 못하게 된다는 점이 다르다. 
commit을 하게되면 rollback으로 정보를 되돌릴 수 없다. 

# OPERATING SYSTEM

# NETWORK
## UDP (User Datagram Protocol), TCP (Transmission Control Protocol)

<img width="648" alt="Screen Shot 2020-07-08 at 1 03 02 PM" src="https://user-images.githubusercontent.com/44011462/86874488-6fab5a80-c11b-11ea-86ef-186410754bff.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
173Page, 그림3-1 트랜스포트 계층은 애플리케이션간의 물리적 통신보다는 논리적 통신을 제공함 <br> 
</details>

### 다중화(Multiplexing)와 역다중화(Demultiplexing)
Transport layer는 전달받은 segment의 data를 application layer의 어떤 socket에 넘겨줄지 확인하기 위해서 segment의 field를 검사하는 것을 말한다. 그리고 이 segment를 해당 socket으로 보낸다. 이를 **Multiplexing**이라고 한다. 반대로 출발지 host에서 socket으로부터 data를 모으고 이에 대한 segment를 생성하기 위해서 각 데이터에 header정보로 캡슐화하고 그 segment를 network layer로 보내는 것을 **Demultiplexing**이라고 한다.

#### 비연결형 다중화

    clientSocket = socket(AF_INET, SOCK_DGRAM);

위와 같은 선언을 통해 비연결형(UDP) socket을 생성할 수 있다. 이 방법으로 socket을 생성할 때 transport layer는 자동으로 port번호를 socket에 할당한다. 하지만 직접 원하는 port 번호를 socket에 할당할 수 있다.

    clientSocket.bind('', 19157);

일반적으로 application의 client는 자동으로 port번호를 할당 받는 것에 반하여, application의 server측은 특정 번호를 할당한다. 이 포트정보를 가지고 segment를 생성하고 application layer에서 multiplexing, demultiplexing 과정을 거쳐 data가 보내지고 되돌아온다. 

<img width="516" alt="Screen Shot 2020-07-08 at 1 43 32 PM" src="https://user-images.githubusercontent.com/44011462/86877082-0f1f1c00-c121-11ea-8568-9f9fb3de8a54.png">
<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
180Page, 그림3-4 출발지와 목적지 포트 번호의 전환  <br> 
</details>

UPD segment에는 목적지의 IP주소와 port번호가 있어서 대상지를 특정할 수 있다. 출발지가 다르더라도 목적지의 주소를 가지고 통신할 수 있다. 이점이 TCP와 다른점이며 이 차이로 인해 여러가지 논리적인 기능의 차이가 생긴다.

#### 연결형 다중화

TCP segment는 목적지 IP주소, 목적지 port번호, 출발지의 IP주소, 출발지 port번호에 의해서 식별된다. host에 TCP segment가 도착하면 위 4가지의 정보를 가지고 multiplexing, demultiplexing을 한다. 

TCP server application은 12000번의 주소를 갖는 welcoming socket을 가지고 있다. 

    clientSocket = socket(AF_INET, SOCK_STREAM);
    clientSocket.connect((serverName, 12000));

server는 host의 연결 요청을 기다리고 있다가 요청을 받으면 앞서 받은 목적지 IP주소, 목적지 port주소, 출발지 IP주소, 출발지 port가 일치하면 segment는 이 socket으로 demultiplexing된다. 

    connectionSocket, addr = serverSocket.accept();

이제 host와 servet는 적절히 통신할 수 있다.

### web-server와 TCP

<img width="587" alt="Screen Shot 2020-07-08 at 2 07 00 PM" src="https://user-images.githubusercontent.com/44011462/86878623-5064fb00-c124-11ea-87c2-6e71c56caf3e.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
183Page, 그림3-5 같은 웹 서버애플리케이션과 통신하기 위해 같은 목적지 포트 번호(80)를 이용하는 두 클라이언트  <br> 
</details>

apache tomcat과 같은 web server는 port 번호 80에서 동작한다. 위 그림은 web server가 각각의 연결에 따라서 새로운 process를 fork하는 것을 보여준다. 이 연결을 통해서 HTTP 요청을 수신하고 HTTP 응답을 전송한다. 그러나 socket과 process가 모두 일대일 대응 관계를 갖는 것은 아니다. 실제로 고성능 웹 서비스들은 하나의 process를 사용하면서 여러개의 **thread**를 생성하여 서비스에 응답한다. 만약 client와 server가 **지속적인(persistent)** HTTP를 사용한다면 해당 기간동안 동일한 socket을 이용하여 통신한다. 이에 반하여 비지속적인(non-persistent) HTTP를 사용한다면 모든 응답/요청마다 새로운 TCP연결이 생성되고 종료되며 이는 overhead에 따른 성능상의 큰 문제가 된다. 

### UDP 

UDP는 transport layer protocol이 할수 있는 **최소 기능**으로 동작한다. UDP는 multiplexing, demultiplexing등의 기능과 간단한 오류검사를 제외하면 IP에 아무것도 추가하지 않는다. 사실 application 개발자가 TCP대신 UDP를 채택하여 개발한다면 application은 거의 IP와 직접 통신하는 셈이다.
UPD는 segment를 송신하기 저에 transport layer사이에서 handshake를 사용하지 않는다. 이런 이유로 UPD를 **비연결형(connectionless)** 이라고 부른다.

**DNS**는 일반적으로 UDP를 사용하는 application layer의 protocol이다. host에서 DNS application query를 생성할 때, DNS query 메세지를 작성하고 UDP에 넘겨준다. 목적지 종단 시스템에서 동작하는 UDP와 host는 어떠한 handshake도 수행 하지 않고 메세지에 header를 추가한 후에 segment를 network layer에 넘겨준다. 이때 query를 보낸 DNS application은 query에 대한 응답을 기다린다. 만약 host가 응답을 수신하지 못하면 query를 다른 Name server로 송신하거나 application에게 응답을 수신할 수 없다는 것을 알려준다.

#### UDP가 TCP보다 좋은점
TCP는 신뢰적인 데이터 전송 (reliable data transfer) 서비스를 제공하지만, UDP는 그렇지 않으므로 항상 TCP가 더 좋을까? 답은 '**아니오**'이다. 다음과 같은 이유에서 UDP를 사용하는 것이 더 적합하다.

**실시간성이 중요한 application**  
UDP에서는 application process가 data를 UPD에게 전달하면 UDP는 data를 UDP segment로 만들어서 바로 network layer로 전송한다. 이에 반하여 TCP는 congestion control을 실시하여 목적지 host와 출발지 host 사이가 과도하게 혼잡하면 전송속도를 조절한다. 또한 TCP는 신뢰적인 전달을 중요시 하므로 응답시간의 빠르기와 상관없이 목적지 host가 segment의 수신여부를 확인할때까지 기다리거나 재송신한다. UDP를 사용하면 조금의 data loss는 발생할 수 있지만, application 개발자가 일부 data loss는 허용할 수 있으므로 TCP는 위의 조건에 맞지 않는다.

**연결 설정이 없다**  
TCP는 data 전송을 시작하기전에 3-way handshake를 사용한다. 반면에 UDP는 형식적인 예비동작없이 data를 전송한다. 그러므로 UDP는 연결을 설정하기 위한 어떠한 지연도 없다. 이 이유로 DNS가 TCP가 아닌 UDP를 사용한다. DNS가 TCP를 사용한다면 많이 느려진다. 하지만 HTTP로 작성된 웹 페이지는 신뢰성이 중요하기 때문에 보통은 TCP로 동작한다. 

**연결 상태가 없다**  
TCP는 종단 시스템에서 연결 상태를 유지한다. 이 연결상태는 수신버퍼, 송신버퍼, 혼잡제어의 변수, 순서번호와 응답확인번호 등등의 정보를 포함한다. 따라서 동일한 조건이라면 TCP보다 UDP가 조금더 많은 client를 보유할 수 있다.

**작은 packet header overhead**  
TCP가 segment마다 20bytes의 header overhead를 갖는 반면에 UDP는 단지 8bytes의 header overhead를 갖는다.

#### UDP segment 구조

<img width="213" alt="Screen Shot 2020-07-09 at 6 05 17 PM" src="https://user-images.githubusercontent.com/44011462/87020400-c34aa080-c20e-11ea-8e21-42dccc9db59f.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
187Page, 그림3-7 UDP 세그먼트 구조<br> 
</details>

#### UDP check-sum

UDP check-sum은 error detection을 제공한다. segment가 출발지에서 목적지로 이동핸 후 UDP segment의 정보가 손실 또는 변경되었는지 확인하는 것이다. 매우 다양한 방식의 UPD check-sum이 존재한다. 그중 보수를 이용하여 모든 자리의 합이 1이 되는지 확인하는 방법이 있다. UDP는 TCP에 비해 가볍고 빠르며 신뢰성을 보장하는 것이 장점으로 여겨질 때가 많다. 또한 많은 링크 계층의 protocol이 error detection을 제공하는데 굳이 UDP가 check-sum을 제공하는 이유는 무엇일까. 바로 출발지와 목적지 사이에 있는 모든 링크가 error detection을 제공하는 것은 아니기 때문이다. 그러므로 segment가 정확하게 도착하였다고 하더라도 비트가 일부분 변경되어 전송되는 것이 가능하다. 이때 UDP header에 있는 check-sum을 확인하여 error를 detect한다. 하지만 이를 고치는 방법(error correction)을 포함하고 있지 않아서 단순히 참고만 할 수 있다. 


### 신뢰적인 데이터 전달(reliable data transfer)의 원리
**비트 오류와 손실 있는 채널 상에어싀 신뢰적 데이터 전송: rdt3.0**  

<img width="528" alt="Screen Shot 2020-07-09 at 6 19 47 PM" src="https://user-images.githubusercontent.com/44011462/87021960-ca72ae00-c210-11ea-939c-b00583924adb.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
199Page, 그림3-15 rdt3.0 송신자<br> 
</details>


비트가 손상되는 것 외에도 하위 채널이 패킷을 손실하는 경우를 생각할 수 있다. 

1. 어떻게 패킷 손실을 검출할 것인가
2. 패킷 손실이 발생했을 때 어떤 행동을 할 것인가

패킷 손실은 송신자에게 손실된 패킷과 회복 책임을 부여하여 해결 가능하다. 송신자가 데이터 패킷을 전송하고 ACK을 손실했다고 가정하자. 송신자에게 응답이 오지 않는다. 만약 송신자가 패킷을 잃어버렸다는 것을 확신할 수 있을 정도로만 기다릴 수 있다면 패킷은 재전송될 수 있다. 

**그렇다면 얼마나 기다려야 하는가??** 송신자는 적어도 송신자와 수신자 사이의 왕복시간 지연에 수신 측에서 패킷을 처리하는데 걸리는 시간을 더한 만큼 기다려야한다. 하지만 이 시간은 예측하기도 어려울 뿐 아니라, 시간을 기다리는 것은 오류 복구가 시작될 떄까지 기다려야하는 시간이 길수도 있다는 점에서 이는 좋지 못한 방법이다. 

실제 네트워크 상황에서는 송신자가 패킷 손실을 헀다는 보장은 없지만 만일 ACK이 일정 시간안에 도착하지 않는다면 재전송한다. 중복 데이터에 대한 해결은 순서번호를 이용하여 가능하다. 

### TCP

TCP는 application process가 data를 다른 process에게 보내기 전에 서로 먼저 "**handshake**"를 하기 때문에 연결 **지향적 (Connection-oriented)** 이다. TCP연결은 회선 교환 네트워크에서와 같은 종단간의 TDM (Time Division Multiplexing)이나 FDM (Frequenct Division Multiplexing)이 아니다. 연결 상태가 두 종단 시스템에만 존재하므로 가상회선도 아니다. TCP protocol은 오직 종단 시스템에만 동작하고 중간의 네트워크 요소에서는 동작하지 않으므로, 중단의 네트워크 요소들은 TCP 연결 상태를 유지하지 않는다. 물론 중간 router들은 TCP연결을 전혀 감지하지 못한다. 

TCP연결은 **전이중(Full-duplex)** 서비스를 제공한다. 또한 TCP 연결은 항상 **점대점(point to point)** 이다. TCP에서는 multicasting이  불가능하다.

    clientSocket.connect((serverName, serverPort));

client가 connect 요청을 보내면 server는 ACK을 담은 segment로 응답한다. 마지막으로 client가 SYN/ACK으로 다시 응답한다. 처음 2개의 segment에는 '**payload**', 즉 apllication layer data가 없다. 세번째 segment는 payload를 포함할 수 있다. 두 host사이에서 3개의 segment가 보내지므로 이 연결설정 절차는 흔히 **'three-way handshake'** 라 부른다.

#### TCP segment 구조

<img width="457" alt="Screen Shot 2020-07-09 at 8 22 20 PM" src="https://user-images.githubusercontent.com/44011462/87033900-fe0a0400-c221-11ea-8854-d543324d21c3.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
199Page, 그림3-29 TCP 세그먼트 구조<br> 
</details>

위 그림은 segment의 구조를 보여준다. UDP와 동일하게 header는 상위 계층 application으로부터 multiplexing과 demultiplexing을 하는데 사용하는 출발지(source)와 목적지(destination) port번호를 포함한다. 또한 UDP처럼 check-sum 필드를 포함한다. UDP와 다른 내용으로 아래와 같은 field를 포함한다.

   1. 32bits **순서번호 필드(SEQuence number filed)** 와 32bits **확인응답번호 필드(ACKnowledgement number field)** 가 있다. 이는 Reliable data transfer 서비스 구현에서 TCP 송신자와 수신자에 의해서 사용된다.
   2. 16bits **수신 윈도우(receive window)** 필드는 congestion control에 사용된다. 이는 수신자가 받을 수 있는 byte의 크기를 나타낸다.
   3. 4bits 헤더 길이(header length) 필드는 TCP header의 길이를 나타낸다. 이는 가변길이이며 일반적으로 20bits로 되어있다.
   4. 옵션(option)필드는 최대 세그먼트 크기(Maxium Segment Size, MSS)를 정하거나 window 확장 요소로 이용된다.
   5. **플래그(flag)** 필드는 6bits를 포함한다. **ACK 비트**는 확인응답 필드에 있는 값이 유효한지 확인하는데 사용한다. 

#### 흐름 제어 (Flow Control)
**흐름 제어(Flow Control)** 는 수신자(Receiver)와 송신자(Sender) 사이에 생기는 전송과 수신의 **속도차이**에 의해 발생한다. 이는 application process가 데이터를 받는 시점과 사용하는 시점이 항상 같은것이 아니기 떄문에 발생하는 문제이다. application process가 다른작업으로 인해 바쁘다면 전송받은 data를 buffer에 담아두고 필요한 시점에 사용한다. 이때 수신자의 data 처리속도가 송신자보다 느리다면 **buffer가 overflow**되어 정소를 소실한다. 이와 같은 문제를 해결하기 위해서 application에 **흐름 제어(Flow Control)** 를 제공한다. 

TCP는 송신자가 **수신 윈도우 (Receive Window)** 를 두어 흐름제어를 제공한다. 수신 윈도우는 수신측에서 가용한 크기가 얼마인지 송신자에게 알려주는데 사용한다. TCP는 전이중(Full-duplex)이기 때문에 수신자와 통신하는 각각의 송신자는 **수신 윈도우rwnd**를 두어 흐름을 통제한다. 

LastByteRead: RcvBuffer로 부터 읽힌 data stream의 마지막 byte의 수  
LastByteRcvd: RcvBuffer에 저장된 data stream의 마지막 byte의 수   

    LastByteRcvd - LastByteRcvd >= RcvBuffer 
    cwnd = RcvBuffer - (LastByteRcvd - LastByteRead)  

위의 내용을 정리하면 아래와 같은 표를 얻을 수 있다.

<img width="391" alt="Screen Shot 2020-07-10 at 2 40 33 PM" src="https://user-images.githubusercontent.com/44011462/87120429-580ad800-c2bb-11ea-9a5e-1f4087fca583.png">


<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
232Page, 그림3-38 수신 윈도우(rwnd)와 수신 버퍼(RcvBuffer)<br> 
</details>

송신자는 LastByteSent, LastByteAcked라는 변수를 두어 흐름 제어에 활용한다. 두 변수의 차이인 LastByteSent - LastByteAcked는 아직 ACK을 받지 못한 data의 양을 뜻한다. 이 값이 rwnd보다 작다면 수신자측의 **RcvBuffer가 overflow되지 않음이 보장**된다.

    LastByteSent - LastByteAcked <= rwnd

이 방식은 수신버퍼가 rwnd = 0으로서 가득 찼다고 가정하면 송신자는 data를 추가로 보내지 못하여 ACK을 받을수가 없게 된다. 이 경우에는 수신자 측에서 RcvBuffer를 비우게 되더라도 송신자측이 알 수 있는 방법이 없다. 따라서 ***rwnd = 0*** 인 상황이 되면 송신자는 segment의 크기가 1byte인 작은 data를 **지속적으로 보내어** ACK을 확인한다. 결과적으로 버퍼는 비워지며 문제를 해결한다.

#### TCP Three-way Handshake

TCP는 UDP와 달리 연결 설정을 하며, 연결 상태를 유지하도록 되어있다. **세방향 핸드쉐이크 (three-way handshake)** 를 통해 TCP의 연결을 설정하고 상태를 유지한다.


<img width="362" alt="Screen Shot 2020-07-10 at 3 16 45 PM" src="https://user-images.githubusercontent.com/44011462/87122848-9e166a80-c2c0-11ea-80b5-4b12c4f826a3.png">

<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
234Page, 그림3-39 TCP 'three-way' handshake: 세그먼트 교환<br> 
</details>


1. client측 TCP는 application layer data를 포함하지 않고 SYN bit라는 flag bit를 포함하는 특별한 segment를 만든다. 이 segment를 **SYN segment**라고 부른다. 이 segment는 IP datagram안에 캡슐화되어 server측으로 보내진다.
2. TCP SYN segment를 포함하는 IP datagram이 server에 도착하면 server는 IP datagram으로부터 TCP SYN segment를 뽑아낸다. 이 정보를 가지고 TCP buffer와 변수를 할당한다. 이 과정후에 SYN에 대한 응답으로 **SYN-ACK segment**를 보낸다. SYN-ACK segment에도 SYN segment와 마찬가지로 application layer data를 포함하지 않는다. 앞서 받은 SYN segment의 header field 중에서 확인응답 필드(acknowledgement number field)의 값을 1만큼 증가시키고, 순서번호 필드 (sequence number field)에 server에 할당된 정보를 설정하여 보낸다. 
3. SYN-ACK segment를 수신하면 client는 확인응답 필드의 값을 1만큼 증가시키고, 앞선 과정을 통해 이미 연결되었기 때문에 SYN은 0으로 설정된다. 이후 application layer data를 payload로 넣어 segment를 보내며 통신한다.

이 과정을 **three-way handshake**라고 부른다. 이 방식은 암벽을 오르는 사람들이 줄을 이용하여 소통하는 방식과 매우 유사하다.
연결을 끝맺음 할때는 segment header field에서 FIN flag를 1로 설정하여 끝을 알린다. 

<img width="404" alt="Screen Shot 2020-07-10 at 3 32 23 PM" src="https://user-images.githubusercontent.com/44011462/87123872-91931180-c2c2-11ea-9052-9644f7220a78.png">

<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
236Page, 그림3-41 클라이언트 TCP에서 TCP 상태 전이의 일반적인 순서<br> 
</details>


#### 혼잡 제어 (Congestion Control)

TCP 흐름 제어는 수신자가 가진 RcvBuffer에서 일어나는 overflow에 의해 data소실이 발생하고, 그로 인한 재전송, 정보손실 등의 overhead를 막는 기능이다. 이에 반하여 TCP 혼잡 제어(Congestion Control)은 네트워크가 혼잡하면 라우터의 buffer에서 overflow가 일어나 정보가 소실되는 것을 막는다. 따라서 network 전체의 흐름을 제어하는 것과 유사한 효과가 일어나고 이를 **TCP 혼잡 제어(Congestion Control)** 이라고 한다. IP layer는 네트워크 혼잡에 대한 종단 시스템에게 어떤 정보도 알려주지 않으므로, TCP는 종단간의 제어방식을 이용한다. 
TCP 혼잡제어는 송신자측에서 **혼잡 윈도우(Congestion Window, cwnd)** 변수를 두어 제어한다. 이 변수의 값을 이용하여 네트워크로 traffic을 발생시킬 수 있는 비율을 제한한다. 특히 송신하는 쪽에서 응답확인 (ACK)을 받지 못한 데이터의 양은 cwnd, rwnd의 최소값을 초과하지 않는다.

    LastByteSent - LastByteAcked <= min(cwnd, rwnd)

##### 슬로 스타트 (Slow Start)

TCP 연결이 시작될 때, cwnd의 값은 일반적을 1MSS로 초기화 되고, 그 결과 초기 전송률은 대략적으로 MMS/RTT가 된다. 예를 들어 만약 MMS = 500 bytes이고 RTT = 200 msec, 이면 초기 전송률은 20 kbps정도가 된다. 슬로 스타는 cwnd값을 1MSS에서 시작하여 ACK을 받을 때 마다 cwnd의 크기를 1MSS씩 증가시킨다. **각 ACK segment에 대해 2개의 "최대-크기" segment를 전송**한다. 이러한 과정을 통하여 TCP 전송률은 작은 값으로 시작하지만 **지수적으로** 증가하게 된다.

**슬로우 스타트 종료 조건 3가지**
- time out에 의한 segment 손실 발생
TCP송신자는 cwnd값을 1MSS로 하여 새로운 slow start를 시작한다.
- ssthresh <= cwnd 인 경우
TCP송신자는 slow start는 중단하고 혼잡회피로 들어간다
- 3개의 duplicated ACK segments
TCP송신자는 slow start는 중단하고 빠른 재전송으로 들어간다.

<img width="317" alt="Screen Shot 2020-07-21 at 3 58 51 PM" src="https://user-images.githubusercontent.com/44011462/88022948-27545980-cb6b-11ea-81fb-65d0312add8f.png">

<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
250Page, 그림3-50 TCP 슬로 스타트<br> 
</details>



##### 혼잡 회피 (Congestion Avoidance)

혼잡 회피 상태로 들어가는 시점에서 ssthresh의 값은 종전의 값의 1/2가 된다. cwnd는 1MSS를 갖는다. 이후에는 조금더 보수적인 자세를 취하여 1MSS만큼 cwnd를 증가시킨다. 

**혼잡 회피 종료 조건 2가지**
- time out에 의한 segment 손실 발생
TCP송신자는 cwnd값을 1MSS로 하여 새로운 slow start를 시작한다.
- 3개의 duplicated ACK segments
빠른회복으로 전환되면서 혼잡 회피가 끝난다. ssthresh는 cwnd의 값을 쓰며 cwnd의 값은 1/2가 된다.


##### 빠른 회복 (Fast Recovery)

빠른 회복에서 duplicated ACK을 수신할 때마다 cwnd의 크기를 1MSS만큼 증가시킨다. 

**빠른 회복 종료 조건 2가지**
- time out에 의한 segment 손실 발생
TCP송신자는 cwnd값을 1MSS로 하여 새로운 slow start를 시작한다.
- 3개의 duplicated ACK segments
빠른회복으로 전환되면서 혼잡 회피가 끝난다. ssthresh는 cwnd의 값을 쓰며 cwnd의 값은 1/2가 된다.

<img width="642" alt="Screen Shot 2020-07-21 at 4 06 26 PM" src="https://user-images.githubusercontent.com/44011462/88023527-253eca80-cb6c-11ea-8191-28e01466c54c.png">

<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
251Page, 그림3-51 TCP 혼잡제어의 FSM 설명<br> 
</details>

# SECURITY
## steganography

**스테가노그래피(steganography)** 는 데이터를 은폐 기술로 그리스어로 감추어져 있다는 뜻인 *'stegano'* 와 쓰다, 그리다는 뜻의 *'graphos'* 의 합성어이다. 이는 사진, 음악, 동영상 등의 일반적인 파일 안에 데이터를 숨기는 기술이다. **크립토그래피(cryptography)** 는 데이터의 내용을 읽을 수 없도록 하는 기술이라면 **strganography**는 데이터의 존재 자체를 감추는 기술이다. 

<img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px>  

*사람의 눈에는 서로 다르지 않은 동일한 파일로 인식된다.*

**steganography가 악용된 실제 사례**
* 미국의 911테러를 이르킨 빈라덴이 테러지령을 *steganography*로 전달한 것이 밝혀지며 유명한 정보 은닉 기술로 알려졌다.
* 한때 악망 높았던 *랜섬웨어(ransomware)* 가 *steganography*를 이용하였다. 이메일을 통해 *window script file(.wsf)* 형식의 악성 스크립트 파일을 첨부하고, 그 해당 *wsf*이 특정 URL로 접속하게하여 이미지 파일을 다운로드시켰다. 그 이미지에는 랜섬웨어 관련 파일들이 *zip*의 형태로 숨겨져 있었다.  
    <img src="https://user-images.githubusercontent.com/44011462/92375171-b5949700-f13b-11ea-9141-1ab9bb6f020a.png" width=300px>  

    *랜섬웨어에 감염된 화면*

* 배너 광고에 *malware*가 숨겨진 사례로, Internet Explorer와 adobe Flash player의 *보안 취약점*을 이용하여 악성 javascript code가 *steganography*를 이용하여 숨겨진 이미지가 배너에 사용되었다.   
    <img src="https://user-images.githubusercontent.com/44011462/92381313-900c8b00-f145-11ea-8fec-7ce8d48ff50e.png" width=300px>  
    
    *스테가노그래피 기법으로 JPG파일에 숨겨져 있는 랜섬웨어 압축 포멧*

### 상용 스테가노그래피(steganopraphy)


[openStegano](https://www.openstego.com/index.html)
개발언어: Java([open source](https://github.com/syvaidya/openstego))
사용환경: Window, Linux

<img src="https://user-images.githubusercontent.com/44011462/92380767-aa923480-f144-11ea-8c41-5ccefa210478.png" width=300px> <img src="https://user-images.githubusercontent.com/44011462/92380858-cac1f380-f144-11ea-8ece-5e3a18f7aefe.png" width=300px>

<img src="https://user-images.githubusercontent.com/44011462/92380942-eb8a4900-f144-11ea-85e5-aa202fd67302.png" width=300px> <img src="https://user-images.githubusercontent.com/44011462/92380993-065cbd80-f145-11ea-9e14-9af3187d274d.png" width=300px>

### steganography 구현

#### 개요
개발환경: window 10, visual studio 2019 
사용언어: c++ 14
프로그램 설명: window bitmap file(.bmp) 형태의 파일에 원하는 message를 숨기고 열어볼 수 있는 프로그램

#### 사용방법
우선 숨기기를 원하는 최대 500자의 text와 bmp확장자를 가진 이미지가 필요하다.
이미지와 실행파일을 동일한 directory에 위치시키고 아래와 같은 명령을 수행한다. 
실행파일의 이름은 stego.exe로 이미지의 이름은 origin.bmp로 지정한다.  


<img src="https://user-images.githubusercontent.com/44011462/93299549-2e78aa80-f830-11ea-835a-9b77f37c3c89.png" width=300px>  <img src="https://user-images.githubusercontent.com/44011462/93299548-2e78aa80-f830-11ea-8b06-68c472185b89.png" width=300px>

*window command를 이용한 encoding과 decoding의 모습*


#### 실행결과

<img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/93299536-2b7dba00-f830-11ea-9f6a-9e440eb4b009.png" width=300px>  


<img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/93299550-2f114100-f830-11ea-8cff-a324be282500.png" width=300px>  

*origin.bmp와 stego.bmp의 모습*

#### 구현원리

24 비트맵 이미지의 경우, bitmap 이미지 데이터는 offset 0x36인 곳에서부터 24 비트맵 이미지의 경우, bitmap 이미지 데이터는 offset 0x36인 곳에서부터 blue, green, red 순서로 된 튜플들을 순차적으로 저장하고 있다. 각 색상의 크기는 1바이트. 따라서, 픽셀당 **3bytes**를 사용한다. 이 각 픽셀이 가진 정보는 *2^24(약 1600만개)* 가지로, 아주 작은 차이는 *사람의 눈으로는 구분이 불가능*하다.
이 점을 이용하여 3byte의 *마지막 비트를 steganography를 위한 공간으로 활용*한다. 따라서 숨길 수 있는 데이터의 크기는 비트맵 헤더파일이 담고 있는 정보중 offset 0x02에 있는 **BMP 파일 크기** 정보에서 헤더 파일의 전체 크기인 **54bytes**를 제외한 크기만큼 가능하다.  
**encoding**은 3bytes중에서 가장 마지막 비트마다 message의 binary정보를 순서대로 넣었다.
**decoding**은 encoding의 반대개념으로, BMP를 탐색하면서 3bytes의 마지막 비트를 차례대로 수집하였다. encoding시에는 다루고자하는 message의 길이르 알수 있었지만, decoding때는 길이를 알수 없어서 최대길이를 1000자로 제한하는 조건을 걸어두어 해결하였다.


**bitmap, BMP의 파일 format**
|offset(dec) |offset(hex)|크기 |목적 |
|:---:|:---:|:---:|---|
|0  |0x0 |2 |**BMP 파일을 식별**하는 데 쓰이는 매직 넘버: 0x42 0x4D (B와 M에 대한 ASCII 코드 포인트)|
|2	|0x2 |4	|**BMP 파일 크기** (바이트 단위)
|10	|0xA |4	|비트맵 데이터를 찾을 수 있는 **시작 오프셋** (바이트 단위)
|14	|0xE |4	|이 헤더의 크기 (40 바이트)
|18	|0x12 |4 |**비트맵 가로** (단위는 화소, signed integer).
|22	|0x16 |4 |**비트맵 세로** (단위는 화소, signed integer).
|54	|0x36 |- |**실제 데이터**


#### 소스코드

<details>
    <summary><span style="color:grey">클릭하여 소스보기</span></summary>

```c++
/*
 * 2020.9.15
 * 컴퓨터보안 project #1 - Steganography
 * 12164720 정경하 컴퓨터공학과
 */
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include <fstream>
#include <bitset>
#include <string>
#include <Windows.h>

using namespace std;

BITMAPFILEHEADER bf;
BITMAPINFOHEADER bi;

const int COLOR_SIZ = 3;
const int MAX_TXT_SIZ = 500;

unsigned char *bmp_img;

string txt;
string txt_in_bin;

void print_message(const string &msg);

void decode();

void read_file(const char *img);

void convert_bin_to_txt();

void encode();

void write_file();

void convert_txt_to_bin();

int main(int argc, char **argv) {

    if (argc != 2) {
        print_message("usage error! usage : mystego.exe e/d");
        exit(-1);
    }

    if (argv[1][0] != 'e' && argv[1][0] != 'd') {
        print_message("usage error! usage : mystego.exe e/d");
        exit(-1);
    }

    switch (argv[1][0]) {
        case 'e':
            encode();
            print_message("encoding success!!");
            break;

        case 'd':
            decode();
            print_message("decoding success!!");
            break;

        default:
            print_message("argument error! argument should be e or d");
    }
    cout << " ...program exit\n";
    return 0;
}


void print_message(const string &msg) {
    cout << msg << endl;
}

void decode() {
    //TODO: BMP파일에 숨겨진 messge를 읽음
    txt_in_bin = "";
    read_file("stego.bmp");

    for (int i = 0; i < bi.biSizeImage; i = i + COLOR_SIZ) {
        txt_in_bin.push_back(bmp_img[i]);
    }
    convert_bin_to_txt();
}

void convert_txt_to_bin() {
    //TODO: 사용자로부터 message를 입력받아 binary 코드로 변환
    cout << "insert message to Steganograph" << endl;
    getline(cin, txt);
    int txt_len = txt.size();
    if (txt_len > MAX_TXT_SIZ) {
        print_message("message is too long. (smaller than 1000 characters)");
        exit(-1);
    }

    for (int i = 0; i < txt_len; ++i) {
        string temp = bitset<8>(txt[i]).to_string();
        for (int j = 0; j < 8; ++j) {
            txt_in_bin.push_back(temp[j]);
        }
    }
}

void read_file(const char *img) {
// TODO: BMP형식의 파일을 읽어 메모리 heap영역에 저장함
    ifstream read_bmp;

    read_bmp.open(img, ios_base::in | ios_base::binary); //a.bmp파일을 바이트로 읽어 들인다..
    read_bmp.read((char *) &bf, sizeof(BITMAPFILEHEADER)); // 사이즈..

    FILE *fileDescriptor = fopen(img, "rb");
    if (fileDescriptor == NULL) {
        print_message("read_file error! cannot read image(.bmp) file");
        exit(-1);
    }

    fread(&bf, sizeof(unsigned char), sizeof(BITMAPFILEHEADER), fileDescriptor);// 사이중 헤더에 사이즈 14바이트..
    fread(&bi, sizeof(unsigned char), sizeof(BITMAPINFOHEADER), fileDescriptor);// 나머지 정보 40바이트 저장..

    bmp_img = new unsigned char[bi.biSizeImage];
    fread(bmp_img, sizeof(unsigned char), bi.biSizeImage, fileDescriptor);

    fclose(fileDescriptor);
}

void encode() {
    //TODO: BMP파일에 message를 숨김

    read_file("origin.bmp");
    convert_txt_to_bin();

    const int MAX_COUNT = txt_in_bin.size();
    int count = 0;
    for (int i = 0; i < bi.biSizeImage, count < MAX_COUNT; i = i + COLOR_SIZ) {
        //TODO: 그림에 binary로 처리한 message를 입력
        bmp_img[i] = txt_in_bin[count++];
    }

    write_file();
}

void write_file() {
// TODO: 파일을 BMP형식으로 내보냄

    FILE *fileDescriptor = fopen("stego.bmp", "wb");
    if (fileDescriptor == NULL) {
        print_message("write_file error! cannot write stego.bmp");
        exit(-1);
    }

    fwrite(&bf, sizeof(unsigned char), sizeof(BITMAPFILEHEADER), fileDescriptor);
    fwrite(&bi, sizeof(unsigned char), sizeof(BITMAPINFOHEADER), fileDescriptor);
    fwrite(bmp_img, sizeof(unsigned char), bi.biSizeImage, fileDescriptor);

    fclose(fileDescriptor);
    delete[] bmp_img;
}


void convert_bin_to_txt() {
// TODO: binary를 글자로 출력함

    int index = 0;
    for (int i = 0; i < bi.biSizeImage; ++i) {
        if (txt_in_bin[index] != '1' && txt_in_bin[index] != '0') break;
        bitset<8> temp(00000000);
        for (int j = 0; j < 8; ++j) {
            temp.set(7 - j, txt_in_bin[index++] == '1');
        }
        cout << (char) temp.to_ulong();
    }
    cout << endl;
}

```
</details>


## AI SUMMIT 2020, SEOUL

[AI Summit 2020 seoul](https://aisummit.co.kr/) COEX

<img src="https://user-images.githubusercontent.com/44011462/93159771-e3846780-f749-11ea-90af-ab196b8b512f.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/93159843-13336f80-f74a-11ea-8a07-c7b64e4e58be.jpg" width=300px>

일시 : 2020년 12월 9-11일  
장소 : Korea seoul, COEX (Grand Ballroom)  
대상 : 보안기업(안랩, 소만사, 이스트시큐리티 등), 국가기관(한국인터넷진흥원, 금융보안원 등), 제조업(현대자동차), 연구기관(대학 등)  
기획/주관 : AI Summit 사무국, DMK  
방식 : 온라인 스트리밍 (현장 등록 50명 선착순)  

### EPP(Endpoint Protection Platform)
주로 클라이언드와 같은 네트워크의 양 끝단에서 보호하는 기술을 일컷는다. *antivirus, data encryption, intrusion prevention*등을 모두 포함하여 말한다.

### EDR(Endpoint Detection and Response)
EPP가 조금더 보완된 형태를 EDR이라고 부르며, 대응방식을 포함한 개념이다. 이 대응방식에는 **Cyber Threat Intelligence(CTI)** 라고 불리는 방식이 있으며 여러 기관, 집단에서 수집한 공격정보를 한곳에 모아 정보를 공유하고 이를 대응에 활용하도록 하는 방식이다. 이때 CTI에 도움이되는 정보는 여러가지가 있으며 대표적으로 *IP Address, C&C server, IOC(Indecator Of Compromise)* 가 있다. 이런 정보를 CTI방식으로 모아 AI를 이용하여 정보를 분석하고 해석한다.

### AI for Malware Detection
AI를 이용한 *malware detection* , 악성 코드 자체를 탐지하는 것보다는 발견된 악성코드를 분류하고 해석하는데 더 효과적으로 알려져 있다. *Ransomware* 등의 일부 특정유형의 악성코드에서는 탐지하는 능력이 매우 뛰어나다. 코드상에서 악성코드임을 알아낼 수 있는 패턴이 존재하기 떄문이다. 하지만 보편적이지 않은 언어로 작성되었거나 packing이 되어있는 경우 등, *training data* 가 많지 않다면 탐지는 어려운 경우가 많다.

### AXI(Explainable AI) for Security
AI를 이용하여 악성코드를 분류하였다면 *deep learning* 모델을 신뢰할 수 있을까. 주로 이미지 처리 분야에서 많이 사용되는 방식으로 *Class Activation Model(CAM)* 이 있다. *deep learning* 의 결과가 실제 전문가(사람)이 보는 것과 비슷하게, '정보로써 의미 있는 영역을 탐지'하는 방식이다. 이런 비슷한 방식을 AI로 악성코드를 탐지하는데 사용되도록 하는 것이 **AXI**이다. AI가 전체 코드중에서 어떤 부분을 읽고 판단했는지를 보여주는 기능이 있다면 더 좋은 **AI for Malware Detection**로 동작할 수 있는 것이다.

### Security for AI
인공지능을 보호하기 위해서, 혹은 인공지능을 공격하기 위해서 사용되는 방식이다.
#### Evasion Attack(Adversarial Example)
이미 trained되어 있는 인공지능 모델의 정보를 이용하여 속이는 방식이다. 사람의 사진에 안경을 씌워 다른 사람으로 인식하게 하거나, 자율주행 자동차에게 표지판의 일부를 바꾸어 사고를 유발하게 하는 방식이다. 
#### Data Poisoning
Evasion Attack이 test단계에서 공격하는 것이라면 **Data Poisoning** 은 *training 단계* 에서 공격하는 기법이다. 대표적인 사례로 마이크로소프트의 Tay라고 불리는 *chatbot* 이다. 2016년 3월 23일에 마이크로소프트사에서 사람의 채팅을 분석하여 특정 연령대의 인물을 모방하도록 하는 서비스의 *chatbot* 이었는데, 최초 16시간의 training과정에서 **의도적으로 잘못된 data**를 입력한 사용자들의 '공격'에 의해 결과적으로 욕설과 인신공격을 일삼는 *chatbot* 이 되어 서비스가 종료된 사례이다.
#### AI Model Stealing
AI Model Stealing은 AI의 구동방식을 이용한 공격이다. 가정에 기가지니를 놓는다면, 동작방식은 회사의 자연어처리 서버를 오고가며 정보를 분석하는 방식이다. 이를 이용하여 의도적으로 임의의 상당히 많은 query를 요청하여 AI model 내부적으로 어떻게 동작하는지 유추하는 방식이다. 이를 **Model Inversion Attack** 이라고 부른다.

<img src="https://user-images.githubusercontent.com/44011462/93162227-3f9dba80-f74f-11ea-9f63-37feb3a0272f.png" width=400px>

자율주행 자동차가 정지 표지판을 보고도 멈추지 않도록 공격하는 시나리오를 작성한다면 아래와 같다.

1. Queries
    수와 종류가 매우 많은 도로교통 표지판의 사진을 보내어 Model inversion attack을 수행한다.
2. Extract Information
    query의 요청과 응답을 비교하여 모델이 가진 class를 유추한다.
3. A proxy of the target AI model
    모델의 class를 보고 취약점(class의 경계가 모호하거나 근접한 부분)을 알아낸다
4. Create Adversarial Examples
    Model이 stop 표지판을 무시하거나 출발하는 신호로 인식할 수 있는 Adversarial Example를 만든다
5. 자율 주행 자동차는 멈추는 신호를 인식하지 못하고 교통사고가 일어난다.

## SQL Injection
SQL injection은 응용 프로그램 보안상의 취약점을 이용하여 개발자가 의도하지 않은 sql을 실행하여 데이터베이스를 비정상적으로 조작하는 code injection attack 방식이다.

**일반적인 웹앱의 호출방법**은 아래와 같다.

    http://www.abc.com/home.php?id=admin&password=1234

**일반적인 mysql을 다루는 php 파일구조**는 아래와 같다.

```php
<?php
  include("./lib/mysql-lib.php");
  mysql_open();
  $query_results = mysql_query("SELECT * FROM usr 
                                WHERE id = `".$_GET['id]"`
                                AND pw = `"$_GET['password']"`");
  $USER_INFO = mysql_fetch_array($query_results);
  ...
?>
```

**정상적인 query**
id가 admin이고 password가 1234인 경우, 아래와 같이 query가 사용되고 query가 참일 경우 로그인에 성공한다.

```sql
SELECT * FROM usr WHERE id = 'admin' AND pw = '1234';
```

**sql injection을 위한 웹앱 호출의 예시**
    
    http://www.abc.com/home.php?id=admin%27%20OR%201%3D1%20--&password=d0be2cd421

**sql injection의 결과 실제 호출외는 sql query**
```sql
SELECT * FROM usr WHERE id = 'admin' OR 1=1 -- AND pw = 'd0be2cd421';
```
'--'sms MySql의 주석(comment)기호이다. 따라서 뒤에 이어지는 Sql query를 무효화한다.
위의 결과는 언제나 참이므로 admin 계정으로 로그인 하게 된다.

**하지만** 일반적인 경우 서버의 소스코드는 공개되지 않기 때문에 해커의 감각이나, 내부구조를 파악할 수 있도록 의도된 공격을 통해 sql injection이 가능하도록 알아내야한다

### blind sql injection
공격하려는 웹앱이 sql injection에 취약하지만 데이터베이스 메세지가 공격자에게 보이지 않을 때 사용하는 공격방식이다. query에 대한  true/false의 응답을 통해 sql injection이 가능하도록 추측한다.

**blind sql injection 예시**

1. 취약점 확인 절차
    ```sql
    SELECT * FROM usr WHERE id = 1 and 1=1 #;
    SELECT * FROM usr WHERE id = 1 and 1=2 #;
    ```
    만약 위 2개의 query결과로 true, false를 얻었다면 위 웹앱은 blind sql injection에 대해 취약점이 있다고 판단 할 수 있다.  

2. 공격 세부절차 1
    ```sql
    SELECT * FROM usr WHERE id = 1 and length(pw)=? #;
    ```
    위 공격의 결과로 얻는 숫자값이 id가 1인 유저의 비밀번호의 길이이다.  

3. 공격 세부절차2
    ```sql
    SELECT * FROM usr WHERE id = 1 and substr(pw,0,1)='a' #;
    ```
    위 공격의 결과가 true이면 비밀번호의 첫번째 글자는 a라는 것을 알 수 있다.  

4. 공격 세부절차3
    모든 경우의 수에 대해 비밀번호를 확인해보면 id가 1인 유저의 비밀번호를 획득할 수 있다.

### sql injection 방어

1. 변수확인
    sql injection을 방어하는 기본적인 방법은 **변수확인**이다. *blind sql injection 공격*을 위한 sql query는 일반적인 query보다 **비정상적으로 긴 경우가 대부분**이다. 변수의 길이를 확인하여 유효성을 체크한다면 상당부분의 sql injection공격을 무력화 하는 것이 가능하다. php의 경우 *mysql_real_escape_string()* 과 같은 함수를 이용할 수 있다.

2. 필터링
    sql query에 사용되는 문자 및 단어들을 전부 **필터링**한다. 공백문자, '#', '=', 'and', 'or', '--'등의 sql **예약어를 사용하지 못하게** 한다.
    1. 필터링을 회파하는 방법 1
        주석기호(/**/)를 이용하여 공백을 제거하는 필터링을 회피한다. sql에 입력되기전에 처리하는 서버언어에서 주석기호를 공백으로 바꾸기 때문에 가능하다.
        ```sql
        /**/UNION/**/SELECT/**/*/**/FROM/**/usr--
        ```
    2. 필터링을 회파하는 방법 2
        char() 함수를 이용하여 문자를 입력하는 방식이다.
        ```sql
        UNION SELECT*(load_file(char(47,112,97,115,115,119,100)))--
        ```
    3. 필터링을 회파하는 방법 3
        16진수 표현을 이용하여 문자를 입력하는 방식이다.
        ```sql
        UNION SELECT*(load_file(0x6333a2f626f6f742e696e69))--
        ```  

3. 비노출 에러메세지
    sql query에 대한 응답의 결과만으로도 상당히 **민감한 정보**가 된다는 것을 알았다. 이런 정보를 사용자에게 **보여주지 않는** 방법이다.  

4. 최소 권한 유저로 데이터베이스 운영
    혹여나 공격자가 sql injection에 성공하더라도 다른 table이나 다른 database에 접근하지 못하도록 **권한을 최소한** 으로 유지해야한다.  

5. 신뢰할 수 있는 네트워크, 서버에 대한 접근 허용
    특정 기관 또는 회사등에서 사용하는 경우라면 **외부의 접근** 자체를 막을 수도 있다. 이는 **공격자가 외부**에서 접근하려는 경우에 사용가능하다.   




# COMPUTER ARCHITECTURE


