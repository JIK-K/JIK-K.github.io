---
layout: post
title: "JAVA - Comparable / Comparator 비교"
categories: JAVA
tags: [Comparable, Comparator, JAVA]
---

## Comparable Interface

<span style = "color:#FF8C00">Comparable Interface</span>는 객체의 정렬에 사용된다. 이 인터페이스를 구현한 클래스는 <span style = "color:#FF8C00">'compareTo()'</span>메소드를 구현해야 한다. compareTo() 메소드는 현재 객체(this)와 비교 대상 객체를 비교 즉 자기자신과 매개변수 객체를 비교하여 음수, 양수, 0을 반환해준다.

```java

class Person implements Comparable<Person> {
    private String name;
    private int age;

    // 생성자, getter, setter 등 생략

    @Override
    public int compareTo(Person other) {
        // 나이를 기준으로 natural order 정의
        return this.age - other.age;
    }
}

```

'Person' 클래스가 Comparable 인터페이스를 구현하였고 compareTo() 메소드를 구현하여 나이를 기준으로 비교를 진행한다.

코드를보면 비교 대상을 자기자신과 매개변수 객체를 비교하는 모습이다.

## Comparator Interface

<span style = "color:#FF8C00">Comparator Interface</span>도 마찬가지로 객체의 정렬, 비교에 사용된다. 이 인터페이스를 구현한 클래스는 <span style = "color:#FF8C00">'compare()'</span> 메서드를 구현해야 한다. compare() 메소드는 두개의 매개변수 객체를 받아 음수, 양수, 0을 반환한다. Comparable과 다르게 Comparator는 두 매개변수 객체를 비교한다는 것이다.

```java

class Person {
    private String name;
    private int age;

    // 생성자, getter, setter 등 생략

    // 나이를 기준으로 정렬하는 Comparator 구현
    static Comparator<Person> ageComparator = new Comparator<Person>() {
        @Override
        public int compare(Person o1, Person o2) {
            return o1.getAge() - o2.getAge();
        }
    };
}

```

'Person' 클래스의 ageComparator 필드에 Comparator<Person> 인터페이스를 구현한 익명 클래스를 할당하여 나이를 기준으로 정렬하는 Comparator를 정의했다.

코드를 보면 비교 대상이 두 매개변수 객체라는 것을 확인할 수 있다. 이후 'Person'객체를 'ageComparator'를 사용하여 나이를 기준으로 정렬할 수 있다.

```java
List<Person> persons = new ArrayList<>();
// persons 리스트에 Person 객체들 추가

Collections.sort(persons, Person.ageComparator); // 나이를 기준으로 정렬

```

위 코드를 보면 'Collections.sort' 메서드를 사용하여 'Person' 리스트의 객체들을 나이를 기준으로 정렬하는 코드이다. 'Collections'에 대해서는 추후 포스팅 예정이다.

'Comparable' 과 'Comparator'는 객체의 비교, 정렬같은 다양한 자료구조나 알고리즘에 활용될 수 있으니. 한번 알아가두고 적어둠으로 기억해야 할 필요가 있다.
