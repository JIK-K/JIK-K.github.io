---
layout: post
title: "알고리즘 - Counting Sort(계수 정렬)"
categories: Algorithm
tags: [Counting Sort, Algorithm, 계수 정렬, 알고리즘]
---

## Counting Sort 이란?

<I><span style = "color:#FF8C00">Counting Sort(계수 정렬)</span>은 정렬 알고리즘 중 하나이며, 입력된 배열의 값들의 개수를 세고 정렬 하는 알고리즘이다.

Counting Sort는 입력 배열의 값의 범위가 비교적 작을때 빠른 속도를 보이는 특정 조건에서만 빠른 속도를 보이는 알고리즘이다.

Counting Sort는 아래와 같은 특징을 가진다.

- 입력된 배열의 값이 정수로 이루어져 있다면 빠른 정렬이 가능하다.
- 입력된 배열의 값의 분포가 일정하지 않다면 속도가 저하될 수 있다.
- 값의 범위가 크다면, Counting Array의 크기가 커지게 되어 메모리 사용량이 많아진다.
- 입력된 배열이 정렬된 이후에도 추가적인 연산을 해야함으로 상대적으로 느린 알고리즘이다.

Counting Sort의 시간 복잡도는 <span style="color:Pink">O(n+k)</span>이다 n은 입력 배열의 크기고, k는 입력된 배열의 최댓값이다.

입력된 배열의 값의 개수를 세어 정렬하는 알고리즘이기 때문에 입력 배열의 크기 n에 비례하여 수행시간이 걸린다. 또한 Counting Array를 생성하고 누적합을 계산하는 과정에서 최댓값 k에 비례하는 시간이 걸린다. 그리하여 배열의 크기와 값의 범위에 따라서 수행시간이 크게 바뀐다.

만약 k가 n보다 작은 경우 Counting Sort의 시작복잡도는 <span style="color:Pink">O(n)</span>으로 축소 될 수 있다.

<hr/>

## Counting Sort 과정

- <span style = "color:Green">1. Counting Array 만들기</span> : 정렬할 배열에서 가장 큰 값을 찾고 그 크기만큼 Counting Array를 만든다.
- <span style = "color:Green">2. Counting Array 채우기</span> : 정렬할 배열의 각 요소를 순회후, 해당하는 Counting Array의 인덱스에 1을 더해준다.
- <span style = "color:Green">3. 누적합 구하기</span> : Counting Array에서 누적합을 계산한다. 누적합은 해당 인덱스까지의 누적 개수를 말한다.
- <span style = "color:Green">4. 정렬된 배열 구하기</span> : 정렬할 배열을 역순으로 순회하고, 해당하는 Counting Array의 누적합 위치에 값을 넣어준후 해당되는 Counting Array의 값을 1 감소 시킨다.

![Counting-Sort](/assets/images/CountingSort.gif)
<small>출처:<https://www.cs.miami.edu/home/burt/learning/Csc517.091/workbook/countingsort.html></small>

<hr/>

## Counting Sort 구현

```java
public static void countingSort(int[] arr) {
    int max = getMax(arr);
    int[] count = new int[max+1];

    // count 배열 채우기
    for(int i=0; i<arr.length; i++) {
        count[arr[i]]++;
    }

    // 누적합 구하기
    for(int i=1; i<count.length; i++) {
        count[i] += count[i-1];
    }

    int[] sortedArr = new int[arr.length];

    // 정렬된 배열 구하기
    for(int i=arr.length-1; i>=0; i--) {
        sortedArr[count[arr[i]]-1] = arr[i];
        count[arr[i]]--;
    }

    // 정렬된 배열로 복사
    System.arraycopy(sortedArr, 0, arr, 0, arr.length);
}

private static int getMax(int[] arr) {
    int max = Integer.MIN_VALUE;
    for(int i=0; i<arr.length; i++) {
        if(arr[i] > max) {
            max = arr[i];
        }
    }
    return max;
}
```

**MAIN**

```java
public static void main(String[] args) {
    int[] arr = {4, 2, 2, 8, 3, 3, 1};
    System.out.println("정렬 전: " + Arrays.toString(arr));
    countingSort(arr);
    System.out.println("정렬 후: " + Arrays.toString(arr));
}
```

**실행 결과**

    정렬 전: [4, 2, 2, 8, 3, 3, 1]
    정렬 후: [1, 2, 2, 3, 3, 4, 8]
