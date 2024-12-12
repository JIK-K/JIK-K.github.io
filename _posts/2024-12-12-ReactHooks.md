---
layout: post
title: "[React]-React Hooks"
categories: React
tags: [React, Typescript]
---

## React Hook

<span style = "font-weight:bold">React Hook</span>은 class없이 React의 기능들 사용할 수 있도록 해준다. 함수형 컴포넌트(Functional Component)가 클래스형 컴포넌트(Class Component)의 기능을 사용할 수 있도록 해준다.

<span style = "font-weight:bold">React Hook</span>의 규칙은 아래와 같다

> - Hook은 조건문이나 반복문, 중첩된 함수 내에서 호출되면 안되고 최상위에서만 호출 되어야 한다.
> - Hook은 함수형 컴포넌트 또는 사용자 정의 훅과 같이 React 함수 내에서만 호출해야한다.

## useState

# 개념

함수형 컴포넌트에서 상태 관리를 위해 사용되는 Hook.
컴포넌트의 내부 상태 관리가 필요하거나, 렌더링 관련 관리가 필요할때나 동적인 UI 업데이트 등에서 사용된다.

이전에는 클래스 컴포넌트에서 state를 사용하여 컴포넌트의 내부 상태를 관리 했었지만 함수형 컴포넌트에서는 state를 직접 사용하는 것이 불가능 하기 때문에 useState가 도입되었다.

# 사용법

useState는 배열을 반환하며, 현재 상태값과 상태를 변경하는 함수가 들어간다.

```typescript
import React, { useState } from "react";

function Counting() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```

## useEffect

# 개념

컴포넌트가 렌더링 된 이후 또는 언마운트 되기 전에 특정 작업을 수행하도록 하는 Hook.
API호출과 같은 비동기 작업을 관리하거나, 메모리 누수를 방지하고 리소스를 확보하기위해서 사용되며 또는 렌더링 이후 DOM 조작과 같은 렌더링 관련 작업을 위하여 사용된다.

# 사용법

useEffect 내부의 return은 해당 effect가 더 이상 실행할 필요가 없을 때 청소하는 용도로 사용된다. effect가 달라져야 할 경우나, 해당 컴포넌트가 언마운트 될때가 해당된다.

```typescript
import React, { useEffect } from "react";
...
// 마운트 될때 한번만 실행
useEffect(() => {
    console.log("useEffect");
    return() => {
        console.log("CleanUp");
    }
},[]);

// 값들이 변경될때 마다 클린업 함수 실행
useEffect(() => {
    consol.log("useEffect");
    return() => {
        console.log("CleanUp");
    };
},[dependency1, dependency2, ...]);

// 매 랜더링 마다 실행
useEffect(() => {
    console.log("useEffect");
    return() => {
        console.log("CleanUp");
    }
});
```

## useRef

# 개념

React 함수형 컴포넌트에서 DOM 요소에 접근하거나 컴포넌트 인스턴스를 참조하는데 사용되는 Hook.
특정 DOM요소를 직접 조작해야하거나, 상태 변경 없이 값이 유지되어야 할때 사용한다.

useRef로 관리하는 값은 값이 변경되어도 리렌더링 되지 않으며, 값의 기록이나 DOM 요소 참조에 사용되는 점이 useState와 다른점이다.

# 사용법

```typescript
import React, { useRef } from "react";

function InputFocus() {
  const inputRef = useRef();

  const focusInput = () => inputRef.current.focus();

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

## useMemo

# 개념

작업 시간과 많은 리소스를 필요로 하는 작업을 메모이제이션하여 재계산을 방지하여 성능을 최적화 하는 Hook.

렌더링이 발생할 때마다 작업 시간과 많은 리소스를 필요로 하는 작업을 수행하게 된다면 성능적인 측면에서 문제가 발생할 수 있으며, useMemo를 사용한다면 이전 계산 결과 값을 캐싱하여 같은 인자가 전달될때 재계산을 하지 않고 이전 계산 값을 반환한다.

# 사용법

```typescript
import React, { useMemo } from "react";

const result = useMemo(() => {
  console.log("Calculating...");
  return count * 2;
}, [count]);
```

## useCallback

# 개념

함수 자체를 메모이제이션하여 성능을 최적화 하는 Hook.

React에서 리렌더링이 발생할 때 함수들이 불필요하게 재생성 되는 경우를 방지하기 위하여 의존성이 변경될 때 마다 새로 생성되도록 제어하기 위하여 사용한다.

# 사용법

```typescript
import React, { useCallBack } from "react";

const increment = useCallback(() => {
  // 계산 또는 작업
  return result;
}, [dependency]);
```

## useReducer

# 개념

컴포넌트의 복장합 상태 관리 로직을 관리 및 처리하기 위한 Hook.

상태 관리가 복잡하고 다양한 상태 변화가 필요한 경우에 사용되며, useState 보다 복잡한 상태 관리에 유리하고 여러 상태를 하나의 리듀서 함수로 관리하여 가독성과 유지보수성을 높이기 위하여 사용된다.

# 사용법

const[state, dispatch] = useReducer(reducer, initialstate);

```typescript
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    default:
      return state;
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
```

## useContext

# 개념

컴포넌트 간에 상태를 공유하고 전달하기 위해 사용되는 Hook.

React는 부모 컴포넌트에서 자식 컴포넌트로 props를 이용하여 단방향으로 흐른다. 이러한 방식이 아닌 컴포넌트 트리 안에서 상태를 공유할때 사용된다.
props로 여러 컴포넌트를 거치지 않고 값을 전달 할 수 있다는 뜻이다.

# 사용법

```typescript
import React, { useContext } from "react";

const ThemeContext = createContext("light");

function ThemedComponent() {
  const theme = useContext(ThemeContext);
  return <p>Theme: {theme}</p>;
}
```
