---
layout: post
title: "알고리즘 - Merge Sort(합병 정렬)"
categories: Algorithm
tags: [Merge Sort, Algorithm, 합병 정렬, 알고리즘]
---

## Merge Sort 이란?

<I><span style = "color:#FF8C00">Merge Sort(합병 정렬)</span>은 비교 기반 정렬 알고리즘이며 분할 정복 알고리즘(Divide and Conquer Algorithm) 중 하나이다.

원소의 개수가 하나 이하가 될 때 까지 분할하고, 분할된 원소들을 정렬하여, 정렬된 원소들을 다시 병합하는 과정을 반복하여 정렬하는 알고리즘이다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">과정</span>

- <span style = "color:Green">1. 분할(divide)</span> 정렬되지 않은 리스트를 절반으로 나누어 두개의 리스트로 만든다.
- <span style = "color:Green">2. 정복(conquer)</span> 각 부분 리스트를 재귀적으로 합병 정렬을 이용하여 정렬한다.
- <span style = "color:Green">3. 결합(combine)</span> 두 부분 리스트를 다시 하나의 리스트로 합병한다. 이떄 정렬 결과가 임시 배열에 저장된다.
- <span style = "color:Green">4. 복사(copy)</span> 임시 배열에 저장된 결과를 원래 배열에 복사한다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">시간복잡도</span>

MergeSort의 시간복잡도는 분할정복(Divide and Conquer) 방법을 사용하기 때문에<span style="color:Pink">O(nlogn)</span>이다.

리스트를 최소 단위의 리스트로 분할하고 정렬해 나가면서 리스트를 분할하는 과정에서 하나의 리스트를 두개의 리스트로 분할하는 과정을 반복하게 되며 이 과정의 시간복잡도는<span style="color:Pink">O(logn)</span>이다.

분할된 리스트를 합치는 과정에서 각 리스트의 원소를 비교하여 정렬하는데, 이때 비교한 원소의 개수는 원소의 개수 n에 비례한다. 이 과정의 시간복잡도는 <span style="color:Pink">O(n)</span>이다.

즉 리스트를 분할 하는데 O(logn), 합치는데 O(n)의 시간복잡도를 가지므로 전체적인 시간복잡도는 <span style="color:Pink">O(nlogn)</span>이 된다.

**Merge Sort는 데이터의 크기가 작을때 보다 큰 경우에 빠른 정렬 효과를 얻을 수 있는 장점이 있다.**

<small>출처:<https://ko.wikipedia.org/wiki/%ED%95%A9%EB%B3%91_%EC%A0%95%EB%A0%AC></small>

<hr/>

## Merge Sort 과정

- <span style = "color:Green">1.</span> : 두 개의 정렬된 리스트 A, B를 비교한다.
- <span style = "color:Green">2.</span> : 두 리스트의 첫 번째 원소를 비교하여 작은 원소를 새로운 리스트에 추가한다.
- <span style = "color:Green">3.</span> : 이후 비교한 원소를 리스트에서 제거한다
- <span style = "color:Green">4.</span> : 1 ~ 3 과정을 반복하면서 리스트가 모두 정렬될때 까지 한다.

![Merge-Sort](/assets/images/MergeSort.gif)
<small>출처:<https://upload.wikimedia.org/wikipedia/commons/thumb/c/cc/Merge-sort-example-300px.gif/220px-Merge-sort-example-300px.gif></small>

<hr/>

## Merge Sort 구현

```java
public class MergeSort {

    public static void main(String[] args) {
        int[] arr = {7, 2, 5, 3, 9, 1};
        System.out.println("Before sorting: " + Arrays.toString(arr));
        mergeSort(arr, 0, arr.length-1);
        System.out.println("After sorting: " + Arrays.toString(arr));
    }

    public static void mergeSort(int[] arr, int start, int end) {
        if (start < end) {
            int mid = (start + end) / 2;
            mergeSort(arr, start, mid);
            mergeSort(arr, mid+1, end);
            merge(arr, start, mid, end);
        }
    }

    public static void merge(int[] arr, int start, int mid, int end) {
        int[] temp = new int[end - start + 1];
        int i = start, j = mid+1, k = 0;

        while (i <= mid && j <= end) {
            if (arr[i] <= arr[j]) {
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
            }
        }

        while (i <= mid) {
            temp[k++] = arr[i++];
        }

        while (j <= end) {
            temp[k++] = arr[j++];
        }

        for (int x = 0; x < temp.length; x++) {
            arr[start+x] = temp[x];
        }
    }
}
```

**실행 결과**

    정렬 전: [7, 2, 5, 3, 9, 1]
    정렬 후: [1, 2, 3, 5, 7, 9]
