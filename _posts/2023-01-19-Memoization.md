---
layout: post
title: "알고리즘 - Memoization(메모이제이션)"
categories: Algorithm
tags: [Memoization, Algorithm, 메모이제이션, 알고리즘]
---

## 메모이제이션 (Memoization) 이란?

<I><span style = "color:#FF8C00">메모이제이션(Memoization)</span>은 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다. <span style = "color:#FF8C00">동적 계획법(Dynamic Programming)</span>의 핵심이 되는 기술이다.</I>

컴퓨터 프로그램이 동일한 계산을 반복한다면 엄청난 중복 호출이 발생할 가능성이 농후하다.

<hr/>

## 피보나치수열로 알아보는 Memoization

메모이제이션을 피보나치 수열 알고리즘으로 설명해보겠다.

피보나치 수열을 구하는 가장 직관적이고 단순한 방법은 아래와 같다.

```java
    int fibo(n){
        if(n <=1){
            return N;
        }
        else{
            return fibo(n - 1) + fibo(n - 2);
        }
    }
```

위와같이 재귀를 이용하는 방식이다. 하지만 재귀적방식을 사용한다면 fibo함수가 재귀적으로 실행되면서 같은 값에 대하여 중복적 계산을 함으로 시간 복잡도가 크다.

하지만 fibo(n)의 값을 계산하자마자 Memoization을 사용하여 저장하면, 시간복잡도를 크게 줄일수 있다

```java
static int[] DP = new int[N]; //배열의 크기는 정의하기 나름이다.

static int memoizationFibo(int n){
    if(n == 0)
        return DP[0];
    if(n == 1)
        return DP[1];
    if(DP[n] != 0)
        return DP[n];
    return DP[n] = memoizationFibo[n - 1] + memoizationFibo[n - 2];
}
```

<b> 즉, Memoization은 중복 계산을 제거함으로 프로그램의 전체적인 실행속도를 증가시키는 기법이다</b>.
