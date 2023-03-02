---
layout: post
title: "자료구조 - Linked List(연결 리스트)"
categories: DataStructure
tags: [Linked List, Data Structure, 연결리스트, 자료구조]
---

## Linked List 란?

<span style = "color:#FF8C00">노드(node)</span>의 집합으로, 각 노드가 <span style = "color:#FF8C00">데이터(data)</span>와 <span style = "color:#FF8C00">포인터(pointer)</span>를
한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료구조(Data Structure)이다.

- <span style = "color:#FF8C00">노드(node)</span>들은 메모리의 어느 위치에나 있을 수 있으며, 다른 노드로 가기 위해서는 현재 노드가 가지고 있는 <span style = "color:#FF8C00">포인터(pointer)</span>를 이용하면 된다.
- 노드는 <span style = "color:#FF8C00">데이터 필드(data field)</span>와 <span style = "color:#FF8C00">링크 필드(link field)</span>로 구성되어 있다.

![DataStructure_LinkedList](/assets/images/LinkedList.jpg)

<hr/>

## Linked List의 종류

- <span style = "color:Green">단순 연결 리스트(singly linked list)</span> : 하나의 방향으로 연결되어 있는 연결 리스트. 체인(chain)이라고도 하며 마지막 노드의 링크는 NULL값을 가진다.
- <span style = "color:Green">원형 연결 리스트(circular linked list)</span> : 단순 연결 리스트와 같으나 마지막 노드이 링크가 첫 번째 노드를 가리킨다.
- <span style = "color:Green">이중 연결 리스트(doubly linked list)</span> : 각 노드마다 2개의 링크가 존재한다. 하나의 링크는 앞에 있는 노드를 가리키며 또 하나의 링크는 뒤에 있는 노드를 가리킨다.

![DataStructure_LinkedList](/assets/images/LinkedList2.jpg)

이번 포스팅에서는 단순 연결 리스트만 다뤄보겠다.

<hr/>

## Node의 구성

노드는 <span style = "color:Green">자기 참조 구조체</span>를 이용하여 정의된다.

자기 참조 구조체란 자기 자신을 참조하는 포인터를 포함하는 구조체이다.

```cpp
typedef int element;

typedef struct ListNode{ //노드의 타입을 구조체로
    element data;
    struct ListNode *link;
}ListNode;
```

<hr/>

## Linked List의 연산

- <span style = "color:Green">insert_first()</span> : 리스트의 시작 부분에 항목을 삽입하는 함수.
- <span style = "color:Green">insert()</span> : 리스트의 중간 부분에 항목을 삽입하는 함수.
- <span style = "color:Green">delete_first()</span> : 리스트의 첫 번째 항목을 삭제하는 함수.
- <span style = "color:Green">delete()</span> : 리스트의 중간 항목을 삭제하는 함수.
- <span style = "color:Green">print_list()</span> : 리스트를 방문하여 모든 항목을 출력하는 함수.

## Linked List의 구현

```cpp
//가장 앞에 노드를 삽입하는 경우
ListNode* insert_first(ListNode *head, int value){
    ListNode *p = (ListNode *)malloc(sizeof(ListNode));
    p->data = value;
    p->link = head;
    head = p;
    return head;
}

//중간에 노드를 삽입하는 경우
ListNode* insert(ListNode *head, ListNode *pre, element value){
    ListNode *p = (ListNode *)malloc(sizeof(ListNode));
    p->data = value;
    p->link = pre->link;
    pre->link = p;
    return head;
}

//첫번째 항목을 삭제하는 경우
ListNode* delete_first(ListNode *head, ListNode *pre){
    ListNode *removed;
    if(head == NULL)
        return NULL;
    removed = head;
    head = removed->link;
    free(removed);
    return head;
}

//중간노드 삭제하는 경우
//pre가 가리키는 노드의 다음 노드를 삭제한다
ListNode* delete(ListNode *head, ListNode *pre){
    ListNode *removed;
    removed = pre->link;
    pre->link = removed->link;
    free(removed);
    return head;
}

//리스트 출력
void print_list(ListNode *head){
    for(ListNode *p = head; p != NULL; p = p->link)
        printf("%d->", p->data);
    printf("NULL \n");
}
```
