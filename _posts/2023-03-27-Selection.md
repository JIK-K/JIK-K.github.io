---
layout: post
title: "알고리즘 - Selection(선택 문제)"
categories: Algorithm
tags: [Selection, Algorithm, 선택 문제, 알고리즘]
---

## Selection 이란?

<I><span style = "color:#FF8C00">Selection algorithm(선택 알고리즘)</span>은 리스트와 같은 자료구조에서 주어진 조건에 따라 특정 위치에 있는 원소를 찾는 알고리즘이다. 즉, n개의 숫자들 중에서 k번째로 작은 숫자를 찾는 문제다.

문제를 해결하기 위한 단순한 방법이 2가지가 있다.

1. 최소 숫자를 k 번 찾는다 (unsotred array) : n개의 숫자들 중에서 k번째 숫자를 찾는 과정이다. k번째 큰수는 n-(k-1)번의 탐색을 하기 때문에 <span style = "color:Pink">시간 복잡도는 O(kn)</span>이다.
2. 숫자들을 정렬한 후, k 번째 숫자를 찾는다 (sorted array) : <span style = "color:Pink">시간 복잡도는 O(nlogn)</span>. 정렬하는 방법에 따라서 걸리는 시간이 다를것.

### 아이디어

이진탐색 : 정렬된 입력의 중간에 있는 숫자와 찾고자 하는 숫자를 비교함으로써, 입력을 1/2로 나눈 두 부분 중에서 한 부분만을 검색

선택문제 : 입력이 정렬 되어 있지 않으므로, 입력 숫자들 중에서 (퀵 정렬에서와 같이) 피봇을 선택하여 아래와 같이 분할.

![Selection](/assets/images/Selection.png)

Small group은 피봇보다 작은 숫자의 그룹이고, Large group은 피봇보다 큰 숫자의 그룹이다. 이렇게 분할 하였을때 각 그룹의 크기를 알면, k 번째 작은 숫자가 어느 그룹에 있는지를 알 수 있으며, 그 다음에는 그 그룹에서 몇 번째로 작은 숫자를 찾아햐 하는지 알 수 있다.

<hr/>

## Selection 과정

<div style = " font-weight:bold">

1. 주어진 배열에서 임의의 원소를 선택한다.(pivot).<br/>

2. pivot보다 작은 원소들과 큰 원소들을 분리한다.<br/>

3. pivot이 위치한 인덱스와 찾고자 하는 원소의 인덱스를 비교한다.<br/>

4. 찾고자 하는 원소가 위치한 인덱스가 pivot의 인덱스와 같다면, 해당 원소를 반환한다.<br/>

5. 찾고자 하는 원소가 위치한 인덱스가 pivot의 인덱스보다 작다면, pivot보다 작은 부분에서 알고리즘을 재귀적으로 실행한다.<br/>

6. 찾고자 하는 원소가 위치한 인덱스가 pivot의 인덱스보다 크다면, pivot보다 큰 부분에서 알고리즘을 재귀적으로 실행한다.<br/>

7. 과정을 반복하여 찾고자하는 원소를 찾을때 까지 반복한다.<br/>

</div>
분할 정복 알고리즘과 방법이 비슷하지만 pivot의 선정방식과 분할방식이 다르다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Selection Pseudo-code</span>

Selection(A, left, right, k)

<span style = "font-weight:bold">입력 : A[left]~A[right]와 k, 단, 1≤k≤|A|, |A|=right-left+1</span>

<span style = "font-weight:bold">출력 : A[left]~A[right]에서 k 번째 작은 원소</span>

1. 피봇을 A[left]~A[right]에서 랜덤하게 선택하고, 피봇과 A[left]의 자리를 바꾼 후, 피봇과 배열
   의 각 원소를 비교하여 피봇보다 작은 숫자는 A[left]~A[p-1]로 옮기고, 피봇보다 큰 숫자는
   A[p+1]~A[right]로 옮기며, 피봇은 A[p]에 놓는다.

2. S = (p-1)-left+1 // S = Small group의 크기

3. if ( k ≤ S ) Selection(A, left, p-1, k) // Small group에서 찾기

4. else if ( k = S +1) return A[p] // 피봇 = k번째 작은 숫자

5. else Selection(A, p+1, right, k-S-1) // large group에서 찾기

<hr/>

## Selection 구현

selection 메서드는 배열 arr에서 k번째 작은 수를 리턴한다.

partiton 메서드는 pivot을 선택하고 pivot보다 작은 원소와 큰 원소를 분리하여 pivot의 위치를 리턴한다.

```java
import java.util.Arrays;
import java.util.Random;

public class App {
    public static void main(String[] args) {
        int[] arr = {6, 3, 11, 9, 12, 2, 8, 15, 18, 10, 7, 14};
        int k = 7;

        int kthSmallest = selection(arr, 0, arr.length - 1, k);
        System.out.println(k + "번째 작은 원소: " + kthSmallest);
    }

    public static int selection(int[] arr, int left, int right, int k) {
        if (left == right) {
            return arr[left];
        }

        int pivotIndex = partition(arr, left, right);
        System.out.println(Arrays.toString(arr));

        if (pivotIndex == k - 1) {
            return arr[pivotIndex];
        } else if (pivotIndex > k - 1) {
            return selection(arr, left, pivotIndex - 1, k);
        } else {
            return selection(arr, pivotIndex + 1, right, k);
        }
    }

    private static int partition(int[] arr, int left, int right) {
        int pivotIndex = new Random().nextInt(right - left + 1) + left;
        int pivotValue = arr[pivotIndex];
        swap(arr, pivotIndex, right);

        int storeIndex = left;
        for (int i = left; i < right; i++) {
            if (arr[i] < pivotValue) {
                swap(arr, i, storeIndex);
                storeIndex++;
            }
        }
        swap(arr, storeIndex, right);

        return storeIndex;
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

```

**실행 결과**

    [6, 3, 2, 7, 8, 11, 14, 15, 18, 10, 9, 12]
    [6, 3, 2, 7, 8, 10, 9, 11, 18, 12, 14, 15]
    [6, 3, 2, 7, 8, 9, 10, 11, 18, 12, 14, 15]
    7번째 작은 원소: 10
