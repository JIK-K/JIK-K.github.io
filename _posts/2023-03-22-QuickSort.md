---
layout: post
title: "알고리즘 - Quick Sort(퀵 정렬)"
categories: Algorithm
tags: [Quick Sort, Algorithm, 퀵 정렬, 알고리즘]
---

## Quick Sort 이란?

<I><span style = "color:#FF8C00">Quick Sort(퀵 정렬)</span>분할 정복 알고리즘(Divide and Conquer Algorithm)중 하나이며 평균적으로 가장 빠른 수행 속도를 가지는 비교정렬에 속하는 정렬 방법이다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">과정</span>

- <span style = "color:Green">1. 분할(divide) : </span> 입력 배열을 피봇을 기준으로 2개의 부분 배열로 분할한다.
- <span style = "color:Green">2. 정복(conquer) : </span> 부분 배열을 정렬한다. 부분 배열의 사이즈가 작다면 재귀 호출을 수행하여 다시 분할 정복 알고리즘을 적용한다.
- <span style = "color:Green">3. 결합(combine)</span> 정렬된 부분 배열을 하나의 배열에 넣는다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">시간복잡도</span>

Quick Sort의 평균 시간 복잡도는 <span style="color:Pink">O(nlogn)</span>이고, Worst Case인 경우 <span style="color:Pink">O(n^2)</span>의 시간 복잡도를 가진다.

피봇의 선정 방식에 따라서 시간 복잡도가 달라지는데 피봇의 선정방법은 전체를 다 비교하여 중간값을 찾거나, 랜덤하게 선정 또는 세개의 숫자를 선정후 중앙값으로 선정하는 방법(Approximately sampling)이 있다.

입력의 크기가 매우 클때는 Quick Sort의 성능을 더 향상 시키기 위하여, 삽입 정렬을 동시에 사용한다. 입력의 크기가 작을때는 퀵 정렬이 피봇 선정의 오버헤드와 재귀호출의 수행으로 삽입 정렬보다 빠르지 않다. 부분문제의 크기가 작아지면 재귀호출을 중단하고 삽입 정렬을 사용한다.

<hr/>

## Merge Sort 과정

- <span style = "color:Green">1. Pivot(선택) : </span> 배열의 첫 번째 원소를 피봇(pivot)으로 선택한다
- <span style = "color:Green">2. Partition 수행 : </span> 피봇을 기준으로 두개의 부분 배열로 나눈다. 피봇보다 작은 값은 피봇의 왼쪽에 큰값은 오른쪽으로 이동하며, 모든 원소가 왼쪽, 오른쪽 배열중 하나에 위치할 때까지 반복한다.
- <span style = "color:Green">3. 재귀적 정렬 : </span> 분할된 두 개의 부분 배열이 각각 Quick Sort 알고리즘을 재귀적으로 호출하여 정렬된다.

![Counting-Sort](/assets/images/QuickSort.gif)
<small>출처:<https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/Sorting_quicksort_anim.gif/220px-Sorting_quicksort_anim.gif></small>

<hr/>

## Merge Sort 구현

```java
class QuickSort
{
    int partition(int arr[], int low, int high)
    {
        int pivot = arr[high];
        int i = (low-1);
        for (int j=low; j<high; j++)
        {
            if (arr[j] <= pivot)
            {
                i++;
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        int temp = arr[i+1];
        arr[i+1] = arr[high];
        arr[high] = temp;
        return i+1;
    }

    void sort(int arr[], int low, int high)
    {
        if (low < high)
        {
            int pi = partition(arr, low, high);
            sort(arr, low, pi-1);
            sort(arr, pi+1, high);
        }
    }

    static void printArray(int arr[])
    {
        int n = arr.length;
        for (int i=0; i<n; ++i)
            System.out.print(arr[i]+" ");
        System.out.println();
    }

    public static void main(String args[])
    {
        int arr[] = {10, 7, 8, 9, 1, 5};
        int n = arr.length;
        System.out.println("정렬 전");
        printArray(arr);
        QuickSort ob = new QuickSort();
        ob.sort(arr, 0, n-1);

        System.out.println("정렬 후");
        printArray(arr);
    }
}
```

**실행 결과**

    정렬 전
    10 7 8 9 1 5
    정렬 후
    1 5 7 8 9 10
