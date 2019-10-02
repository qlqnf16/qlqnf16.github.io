---
title: '알고리즘 베이직'
category: dev
---
# big O notation

똑같은 결과를 내는 함수라도, 어떻게 짜느냐, 실제로 이 함수가 어떻게 동작하느냐에 따라 각각의 함수는 다른 퍼포먼스를 보일 수 있다. 예를 들어 10000 개의 요소가 있는 배열에서 한 요소를 검색한다 할 때, 어떤 함수는 10 초가 걸리기도 하고 어떤 함수는 1 초도 걸리지 않는다. 이런걸 시간복잡도라고 하는데, 이 시간복잡도를 표현하는 방법이 **big O notation** 이다.

1 부터 주어진 수까지 모든 수의 합을 계산하는 함수를 만든다고 가정하자.

```js
// 1
const sum = n => {
  let result = 0;
  for (let i = n; i > 0; i--) {
    result += i;
  }
  return result;
};

// 2
const sum = n => n(n * 1) / 2;
```

위 두 함수는 모두 같은 결과를 내지만, 1 의 경우 for loop 가 n 번 돌아간다. result 에 n, n-1, n-2,..., 1 을 더하는 식으로. n 이 5 면 5 번, 10000 이면 10000 번 돌아가게 될 것이다. 그리고 처음에 result 라는 변수를 지정해주는 시행 한 번을 더하면 이 함수는 n+1 번 돌아간다.

이 때의 시간복잡도는 뒤의 상수를 떼고 n 으로 단순화시켜 **O(n)** 으로 표현한다. for loop 가 각각 두 번 돌아갈때, 그래서 2n 번 돌아갈때도 단순화해 O(n)으로 고정 표기한다.

반면 2 의 경우 숫자가 주어지면 그에 맞는 수학을 딱 한 번만 시행한다. 숫자가 얼마나 큰가에 상관없이 한 번만 돌아가게 되는데, 이 때의 시간복잡도를 **O(1)**로 표현한다.

```js
someFunction = arr => {
  for (i = 0; i < arr.length; i++) {
    for (j = 0; j < arr.length; j++) {
      // ...
    }
  }
};
```

위와 같이 for loop 안에서 또 다른 for loop 가 돌아가는 경우 배열의 크기에 따라 n\*n 번 돌아간다. 그럴 때는 **O(n^2)**로 표현한다.

그 외에도 O(n log n)과 같은 표현방식이 있고, 당연히 더 작을수록 시행 시간이 짧아지기 때문에 더 이상적이다. 함수를 짤 때 그냥 짜는 게 아니라 이 시간복잡도를 고려해 더 효율적으로 짜는 게 좋다는 것!

# Problem solving patterns

# Recursion

# Searching

## Linear Search

배열의 모든 요소를 따라 한 바퀴 돌면서 찾아내는 방식. 자바스크립트에 내장된 .findIndex(), .includes() 등과 같은 함수가 linear search 로 작동한다.

```js
function linearSearch(arr, num) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === num) {
      return i;
    }
  }
  return -1;
}

/// O(n)
```

## Binary Search

정렬된 배열에서만 작동한다. Divide and Conquer 의 방식으로, 시작지점과 끝지점, 그 중간포인트를 정해 계속 반으로 자르면서 찾는다. 배열의 모든 원소를 다 체크해야되는 linear search 와 달리 반으로 쪼개면서 찾기 때문에 처리속도가 훨씬 빠르다.

```js
function binarySearch(arr, val) {
  let left = 0,
    right = arr.length - 1,
    middle = Math.floor((left + right) / 2);
  while (left < right && middle !== val) {
    if (arr[middle] > val) right = middle - 1;
    else left = middle + 1;
    middle = Math.floor((left + right) / 2);
  }
  return middle === val ? middle : -1;
}

/// O(log n)
```

# Sorting

알파벳순, 숫자순 등 우리가 원하는 기준에 따라 요소들을 정렬하는 것.

insertion sort, bubble sort, selection sort 등 다양한 정렬 접근법이 있는데, 각각 장단점이 있고 상황에 따라 퍼포먼스의 속도가 달라진다. 따라서 각각의 정렬법을 배우고, 상황에 맞춰 사용할 필요가 있다.

## Bubble Sort

많이 쓰는 알고리즘은 아님. 효율성의 측면에서 다른 알고리즘보다 떨어지기 때문. 그러나 원리가 직관적이기 때문에 이해하기는 쉽다고 한다. 첫 번째와 두 번째 요소끼리 비교하고, 둘 중 큰 요소를 뒤로 보낸다. 그리고 두 번째와 세 번째 요소를 비교하고, 모든 요소가 정렬될 때까지 반복한다.

아래와 같이 구현할 수 있다.

```js
function bubbleSort(arr) {
  let noSwaps;
  for (let i = arr.length; i > 0; i--) {
    noSwaps = true;
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        noSwaps = false;
      }
    }
    if (noSwaps) break;
  }
  return arr;
}

/// O(n^2)
```

## Selection Sort

요소 중 가장 작은 값을 찾아 첫 번째 요소와 자리를 바꾸는 방식의 정렬 알고리즘. 그 후 다시 남은 요소 중 최소값을 찾아 두 번째 요소와 자리를 바꾸고, 모든 요소가 정렬될 때까지 반복한다.

Bubble Sort, Insertion Sort 와 같은 Time Complexity 를 가지지만(n^2), 거의 정렬이 완료된 상태에서 돌릴 경우 (best) 가장 낮은 퍼포먼스를 보인다(다른 건 n, 얘는 n^2). 정렬이 얼마나 됐느냐에 상관없이 배열의 모든 요소를 따라 반복하기 때문.

아래와 같이 구현할 수 있다.

```js
function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minimum = i;
    for (let j = i; j < arr.length; j++) {
      if (arr[j] < arr[minimum]) minimum = j;
    }
    if (i !== minimum) [arr[i], arr[minimum]] = [arr[minimum], arr[i]];
  }
  return arr;
}

/// O(n^2)
```

## Insertion Sort

요소들을 하나하나 비교하면서 정렬에 맞는 포지션으로 삽입하는 방식. [5, 3, 4, 1, 2] 가 있다고 했을 때, 5 와 3 을 비교해서 [3, 5, 4, 1, 2]로 만들고, 그 후 4 와 3,5 를 비교해 [3, 4, 5, 1, 2]로 만들고 계속 반복하는 방식.

```js
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let currentVal = arr[i];
    for (var j = i - 1; j >= 0 && arr[j] > currentVal; j--) {
      arr[j + 1] = arr[j];
    }
    arr[j + 1] = currentVal;
  }
  return arr;
}

/// O(n^2)
```

위와 같이 구현할 수 있는데, colt 는 모든 변수를 var 로 정의했다. for loop 밖에서 arr[j+1]에 새로운 값을 할당하려고 하기 때문인데, var 안 쓰고 let 가지고 하려면 어떻게 하면 되는 걸까..?

## Merge Sort

여기부터는 앞의 세가지 알고리즘보다 훨씬 더 빠르지만, 더 구현하고 이해하기 복잡한 알고리즘이다. 많은 양의 데이터를 관리할 때 효율적이다. 앞의 세 가지 알고리즘의 시간 복잡도가 O(n^2) 였다면 여기부터의 알고리즘은 O(n log n)이다.

### Merge Sort

1. 한 배열이 주어지면 그걸 모두 쪼개서 요소가 한 개인 각각의 배열로 만든다.
2. 두 개의 배열을 비교해 정렬해서 합치고, 다른 두개를 비교해 정렬해서 합치기를 반복한다. 그 다음에 합쳐진 두 개를 또 비교해서 합치고 ... 끝까지 반복한다.

[1, 4, 7, 3, 9, 8]

[1][4] [7][3] [9][8]

[1, 4][3, 7] [8, 9]

[1, 3, 4, 7][8, 9]

[1, 3, 4, 7, 8, 9] ...이런 과정으로

구현은 다음과 같이 한다

```js
function merge(arr1, arr2) {
  let results = [],
    i = 0,
    j = 0;
  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) {
      results.push(arr1[i]);
      i++;
    } else {
      results.push(arr2[j]);
      j++;
    }
  }
  while (i < arr1.length) {
    results.push(arr1[i]);
    i++;
  }
  while (j < arr2.length) {
    results.push(arr2[j]);
    j++;
  }
  return results;
}

function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  let mid = Math.floor(arr.length / 2);
  let left = mergeSort(arr.slice(0, mid));
  let right = mergeSort(arr.slice(mid));
  return merge(left, right);
}

mergeSort([10, 24, 76, 73]);
```

정렬된 두 배열을 정렬해서 합치는 함수 merge()를 먼저 정의하고 그 다음에 재귀함수 mergeSort()를 이용하는 방식이다. 재귀는 너 무 어렵당 ㅎㅎ,, 나중에 나보고 구현하라면 할 수 있을까 ,,?

## Quick Sort

배열 안에서 첫 번째 요소를 pivot 으로 선택한다. 그리고 나머지 요소를 훑으며 pivot 보다 작은 요소는 pivot 의 왼쪽으로, 큰 요소는 오른쪽으로 보낸다. 왼쪽 부분에서 다시 새로운 pivot 을 설정해서 같은 과정을 반복하고, 정렬이 완료되면 오른쪽 부분에서 pivot 을 설정해서 정렬이 완료될 때까지 반복한다.

아래와 같이 구현할 수 있다.

```js
function swap(arr, swapIdx, i) {
  [arr[swapIdx], arr[i]] = [arr[i], arr[swapIdx]];
}

function pivot(arr, start = 0, end = arr.length + 1) {
  let base = arr[start];
  let swapIdx = start;
  for (let i = start + 1; i < arr.length; i++) {
    if (arr[i] < base) {
      swapIdx++;
      swap(arr, swapIdx, i);
    }
  }
  swap(arr, start, swapIdx);
  return swapIdx;
}

function quickSort(arr, left = 0, right = arr.length - 1) {
  if (left < right) {
    let pivotIndex = pivot(arr, left, right);
    quickSort(arr, left, pivotIndex - 1);
    quickSort(arr, pivotIndex + 1, right);
  }
  return arr;
}

/// O(n log n)
```

## Radix Sort

숫자만 가능한데, 수의 크기를 직접적으로 비교하는 게 아니라 각각의 자리수를 정렬하는 방식으로 이루어진다. 비교를 이용해 정렬하는 앞의 모든 정렬 알고리즘보다 빠르다고 한다. 단 숫자만 가능하다는 거. 속도는 짱짱 빠르다고 한다.

구현은 이렇게

```js
function getDigit(num, digit) {
  num += "";
  return num[num.length - digit - 1] * 1;
}

function digitCount(num) {
  return num.toString().length - 1;
}

function mostDigits(arr) {
  let max = 0;
  arr.forEach(e => (max = Math.max(max, digitCount(e))));
  return max;
}

function radixSort(arr) {
  let mostdigit = mostDigits(arr);
  for (let k = 0; k < mostdigit; k++) {
    let digitbucket = [[], [], [], [], [], [], [], [], [], []];
    arr.forEach(e => digitbucket[getDigit(e, k)].push(e));
    arr = [].concat(...digitbucket);
  }
  return arr;
}
```
