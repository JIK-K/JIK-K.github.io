---
layout: post
title: "알고리즘 - Fractional KnapSack Problem(부분 배낭 문제)"
categories: Algorithm
tags: [Fractional KnapSack, Algorithm, 부분 배낭, 알고리즘]
---

## Fractional KnapSack Problem 이란?

<I><span style = "color:#FF8C00">Fractional KnapSack(부분 배낭 문제)</span>는 물건을 부분적으로 배낭에 담을 수 있으므로, 최적해를 구해서 '욕심을 내어' 단위 무게당 가장 값나가는 물건을 배낭에 넣고, 계속해서 그 다음으로 값 나가는 물건을 넣는다. 만약에 그 다음으로 값나가는 물건을 '통째로' 배낭에 넣을 수 없게 되면, 배낭에 넣을 수 있을 만큼만 물건을 부분적으로 배낭에 담는다.

## Fractional KnapSack Problem 과정

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Fractional KnapSack Problem Pseudo-code</span>

    입력 : n개의 물건, 각 물건의 무게와 가치, 배낭의 용량 C
    출력 : 배낭에 담은 물건 리스트 L, 배낭에 담은 물건 가치의 합 v

    1. 각 물건에 대해 단위 무게당 가치를 계산
    2. 물건들을 단위 무게당 가치를 기준으로 내림차순으로 정렬, 정렬된 물건 리스트를 S
    3. L=∅, w=0, v=0 // L : 배낭에 담을 물건 리스트, w : 배낭에 담긴 물건들의 무게의 합, v : 배낭에 담긴 물건들의 가치의 합
    4. S에서 단위 무게 당 가치가 가장 큰 물건 x를 가져옴
    5. while( (w + weight(x)) ≤ C ){
    6. x를 L에 추가
    7. w = w + weight(x)
    8. v = v + value(x)
    9. x를 S에서 제거
    10. S에서 단위 무게당 가치가 가장 큰 물건 x를 가져옴 }

<hr/>

## Fractional KnapSack Problem 구현

```java
import java.util.*;

class Item {
    int weight;
    int value;
    double valuePerWeight;

    public Item(int weight, int value) {
        this.weight = weight;
        this.value = value;
        this.valuePerWeight = (double) value / weight;
    }
}

public class App {

    public static List<Item> knapsack(List<Item> items, int capacity) {
        // 물건들을 단위 무게 당 가치를 기준으로 내림차순으로 정렬
        Collections.sort(items, new Comparator<Item>() {
            @Override
            public int compare(Item o1, Item o2) {
                return Double.compare(o2.valuePerWeight, o1.valuePerWeight);
            }
        });

        List<Item> knapsackItems = new ArrayList<>();
        int currentWeight = 0;
        int totalValue = 0;

        // 물건들을 배낭에 담는 과정
        for (Item item : items) {
            if (currentWeight + item.weight <= capacity) {
                // 배낭에 물건을 통째로 담을 수 있는 경우
                knapsackItems.add(item);
                currentWeight += item.weight;
                totalValue += item.value;
            } else {
                // 배낭에 물건을 부분적으로 담을 수 있는 경우
                double remainingCapacity = capacity - currentWeight;
                double fractionalValue = item.valuePerWeight * remainingCapacity;
                knapsackItems.add(new Item((int) remainingCapacity, (int) fractionalValue));
                totalValue += fractionalValue;
                break;
            }
        }

        return knapsackItems;
    }

    public static void main(String[] args) {
        // 입력 예시
        List<Item> items = new ArrayList<>();
        items.add(new Item(50, 5));
        items.add(new Item(10, 60));
        items.add(new Item(25, 10));
        items.add(new Item(15, 75));
        int capacity = 40;

        List<Item> knapsackItems = knapsack(items, capacity);

        System.out.println("Knapsack Items:");
        for (Item item : knapsackItems) {
            System.out.println("Weight: " + item.weight + ", Value: " + item.value);
        }

        int totalValue = 0;
        for (Item item : knapsackItems) {
            totalValue += item.value;
        }

        System.out.println("Total Value: " + totalValue);
    }
}

```

**실행 결과**

    Knapsack Items:
    Weight: 10, Value: 60
    Weight: 15, Value: 75
    Weight: 15, Value: 6
    Total Value: 141
