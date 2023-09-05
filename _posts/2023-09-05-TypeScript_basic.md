---
layout: post
title: "[TypeScript] - 타입스크립트 기본 정리"
categories: TypeScript
tags: [TypeScript, 타입스크립트]
---

## 🚩TypeScript 란?

<I><span style = "color:#FF8C00">타입스크립트(TypeScript)</span>는 <span style = "color:#FF8C00">자바스크립트(JavaScript)</span>를 기반으로 <span style = "color:#FF8C00">정적 타입 문법</span>을 추가한 프로그래밍 언어이다.

TypeScript는 정적 타입을 명시 할 수 있다는 점에서 JavaScript와 큰 차이점을 보인다. 그리하여 개발 도구를 사용하면서 개발자가 의도한 변수나 함수의 목적을 명확하게 함으로 의도를 정확하게 전달 할 수 있다.

JavaScript 같은 경우는 runtime에서 에러가 발생하여 프로그램이 실행되다 죽는 경우가 있는데 만약 프로젝트가 큰 경우에는 그런 오류의 수정이 힘들다. 하지만 TypeScirpt는 compile과정에서 에러가 발생하기 때문에 전자와 같은 불상사를 방지 할 수 있다. type을 명시함으로써 compile 단계에서 선언될 수 없는 변수들을 차단하기 때문이다.

TypeScript의 코드는 JavaScript로 변환되며 즉, JavaScript가 실행되는 모든곳에서 사용할 수 있다.

<hr/>

## 1. 기본 타입

# 1-1 문자열

```TypeScript
let hello: string = "Hello World!";
```

# 1-2 숫자

```TypeScript
let one: number = 1234;
```

# 1-3 배열

```TypeScript
let arr1: number[] = [1, 2, 3];
let arr2: Array<number> = [1, 2, 3];
let arr3: Array<string> = ["A", "B"];
let arr4: [string, number] = ["JIK", 25];
```

# 1-4 객체

```TypeScript
let manly: object = {name: "dnswlr", age: 14};
let human: {name: string; age: number} = {
    name: "dnswlr",
    age: 14
}
```

# 1-5 불린(Boolean)

```TypeScript
let flag: boolean = true
```

<hr/>

## 2. 함수

# 2-1 함수 타입

Parameter와 return 값의 타입을 정할 수 있다.

```TypeScript
function add(x: number, y: number) : number{
    return x + y;
}
```

# 2-2 선택적 매개변수

Optional Parameter는 '?' 을 추가해주면 된다

```TypeScript
function createString(str1: string, str2?: string){
    if(str2)
        return str1 + " " + str2;
    else
        return str1
}

let test1 = createString("test");
let test2 = createString("test", "function", "yaya"); //ERROR
let test3 = createString("test", "function");
```

<hr/>

## 3. 인터페이스

Interface는 타입 체크를 위해 사용되며 변수, 함수, 클래스에 사용할 수 있다. 여러가지 타입을 갖는 property로 이루어진 새로운 타입을 정의하는 것.

# 3-1 Interface 선언

```TypeScript
interface User{
    age: number;
    name: string;
}
```

# 3-2 변수 활용

```TypeScript
const manly: User = {name: "dnswlr", age: 25}
```

# 3-3 함수 인자 활용

```TypeScript
function getUser(user: User){
    console.log(user);
}

getUser({name: "dnswlr", age: 25});
```

# 3-4 함수 구조 활용

```TypeScript
interface Add{
    (x: number, y:number) : number;
}

let addFunc: Add = (a, b) => a + b;

console.log(addFunc(12, 6));
```

# 3-5 배열 활용

```TypeScript
interface stringArr{
    [index: number] : string;
}

let arr: stringArr = ["a", "b", "c"];
```

# 3-6 객체 활용

```TypeScript
interface Object{
    [key: string] : string;
}

const object: Object {
    person1: "manly",
    person2: "dnswlr"
}
```

# 3-7 Interface 확장

```TypeScript
interface Person{
    name: string;
    age: number;
}

interface Developer extends Person{
    position: string;
}

const manly: Developer = {
    name: "dnswlr",
    age: 25,
    position: "BE"
};
```

<hr/>

## 4. 타입(Type)

Type은 별칭을 부여하는 것, Type은 Interface와 달리 확장이 불가능 하다.

# 4-1 타입 선언

```TypeScript
type strOrNum = string | number;

const str1: strOrNum = "Hello World!";
const str2: strOrNum = 12345;
```

<hr/>

## 5. 연산자(Operator)

# 5-1 유니온 타입 (Union Type)

한 개 이상의 Type을 선언할 때 사용.

```TypeScript
function strOrNum (value: string | number) {
  if(typeof value === 'string') {
    value.toString();
  } else if(typeof value === 'number') {
    value.toLocaleString();
  } else {
    throw new TypeError('문자열 또는 숫자를 넣어주세요!');
  }
}

strOrNum("Hello World!");
strOrNum(777);
```

# 5-2 교차 타입 (Intersection Type)

명시한 Type을 모두 제공해야한다.

```TypeScript
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skill: string;
}

type Capt = Person & Developer;

let devPerson: Capt = {
  name: "kim",
  age: 777,
  skill: "FE"
};
```

<hr/>

## 6. 클래스 (Class)

# 6-1 접근 제한자

- public : default값, 어디에서나 사용 가능
- protected : 상속받은 하위 클래스만 사용 가능
- private : 선언한 클래스 내에서만 접근가능

| 접근가능성       | public | protected | private |
| ---------------- | ------ | --------- | ------- |
| 클래스 내부      | O      | O         | O       |
| 자식 클래스 내부 | O      | O         | X       |
| 클래스 인스턴스  | O      | X         | X       |

# 6-2 클래스 선언

```TypeScript
class Person{
    private name: string;
    public age: number;
    readonly log: string;  //읽기 전용 property

    constructor(name: string, age: number){
        this.name = name;
        this.age = age;
    }
}
```

# 6-3 상속 (Inheritance)

```TypeScript
// TS
class Input {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    inputName() {
        console.log(`input name is ${ this.name }`);
    }
}
class Button extends Input {
    constructor(name: string) {
        super(name);
    }
}
const button = new Button('click me');
```

# 6-4 오버라이드 (Override)

```TypeScript
class Input {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    inputName() {
        console.log(`input name is ${ this.name }`);
    }
}
class Button extends Input {
    constructor(name: string) {
        super(name);
    }
    inputName() {
        console.log(`button name is ${ this.name }`);
    }
}
const button = new Button('click me');
button.inputName();
```

<hr/>

## 7. Enum

열거형으로 이름이 있는 상수들의 집합을 정의할 수 있다.

# 7-1 숫자형

```TypeScript
enum Students{
    King,   // 0
    mingky, // 1
    nick    // 2
}

const myStudents = Students.mingky; // 1
```

# 7-2 문자형

```TypeScript
enum Player{
    kim = '김',
    park = '박'
}

const player = Player.kim; // 김
```

<hr/>

## 8. 제네릭 (Generics)

단일 타입이 아닌 다양한 타입에서 작동하는 컴포넌트를 작성할 수 있다.

# 8-1 제네릭 선언

```TypeScript
function logText<T>(text: T): T{
    console.log(text);
    return text;
}

logText<string>("Hello World!");
```

# 8-2 Interface Genercis

```TypeScript
interface Menu<T>{
    value: T;
    price: number;
}

const kimbap: Menu<string> = {value: 'kimbap', price: 18000};
```

# 8-3 제네릭 타입 변환

```TypeScript
//배열 힌트
function textLength<T>(text: T[]): T[] {
    console.log(text.length);
    return text;
}

textLength<string>(['hello', 'world']);

//Extends
interface LengthType {
    length: number;
}

function logTextLen<T extends LengthType>(text: T): T {
    console.log(text.length);
    return text;
}

logTextLen('hello world'); // 11
logTextLen(100); // 에러!
logTextLen({ length: 100 }); // 100

//KeyOf
interface Item {
    name: string;
    price: number;
    stock: number;
}

function getItemOption<T extends keyof Item>(itemOption: T): T {
    return itemOption;
}

// 'name', 'price', 'stock'만 인자로 사용 가능
getShoppingItemOption('price');
```
