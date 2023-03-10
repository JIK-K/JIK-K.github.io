---
layout: post
title: "알고리즘 - Insertion Sort(삽입정렬)"
categories: Algorithm
tags: [Insertion Sort, Algorithm, 삽입 정렬, 알고리즘]
---

## Insertion Sort 이란?

<I><span style = "color:#FF8C00">Insertion Sort(삽입 정렬)</span>은 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입하는것으로 졍렬하는 알고리즘이다.

<hr/>

## Insertion Sort 개념

하나씩 배열의 원소를 정렬된 부분과 비교하여 적절한 위치에 삽입하는 방식인데, 구체적인 방식은 아래와 같다.

- <span style = "color:Green">1. </span>첫 번째 원소는 정렬된 것으로 간주한다.
- <span style = "color:Green">2. </span>두번째 원소부터 정렬된 부분과 비교하면서 적당한 위치에 삽입한다.
- <span style = "color:Green">3. </span>삽입하려는 원소와 정렬된 부분의 마지막 원소를 비교한다.
- <span style = "color:Green">4. </span>삽입하려는 원소가 정렬된 부분의 마지막 원소보다 작으면 삽입하려는 원소와 마지막 원소를 교환한다.
- <span style = "color:Green">5. </span>이전 원소와도 비교하여 삽입한다.
- <span style = "color:Green">6. </span>배열의 끝까지 반복한다.

![Insertion-Sort](/assets/images/InsertionSort.gif)

검은색 부분은 이미 정렬된 부분을 나타내며, 빨간색 부분은 삽입 하려는 원소를 나타낸다.

이 과정을 반복하면 최종적으로 정렬된 배열이 나오게 된다.

<span style="color:pink">Insertion Sort(삽입 정렬)</span>의 시간복잡도는 <span style="color:pink">Selection Sort(선택 정렬)</span>과 같이 <span style="color:pink">O(n^2)</span>의 시간복잡도를 가진다.

대부분의 상황에서는 비효율적 이지만 이미 정렬되어 있는 상황이거나, 간단하고 작은배열이거나, 비교하는 횟수가 적어지면 더 빠르게 작동하는 장점이 있다.

<hr/>

## Insertion Sort 구현

java로 구현해보았는데, 배열의 첫 번째 원소를 정렬된 부분으로 간주하고 두번째 원소부터 정렬된 부분과 비교하며 삽입한다

```java
public static void insertionSort(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

key는 현재 삽입하려는 원소를 나타내며, j가 정렬된 부분의 끝 인덱스를 나타낸다.

while 루프문을 타면서 key와 배열의 원소를 비교하며 key가 삽입될 위치를 찾는다. 위치를 찾은후 key보다 큰 원소들을 배열의 한 칸 오른쪽으로 이동하며 그후 key를 삽입한다.
