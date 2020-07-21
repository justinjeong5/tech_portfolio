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
