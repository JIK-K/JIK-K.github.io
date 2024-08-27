---
layout: post
title: "[React]- React isComposing, 한글 두번 입력되는 버그"
categories: React
tags: [React, Typescript]
---

## 원인

React에서 input 태그를 이용하여 한글을 입력할때, 두번 입력되는 경우가 발생한다. 이는 한글이 자음과 모음으로 이루어져 있기 때문이다.
그리하여 한글을 입력할때 즉, 자음과 모음을 입력할 때, IME가 입력중임을 나타내는 isComposing 상태가 된다.
이때 다른 키가 입력되면 IME는 해당키를 모음으로 인식하여 한글자로 처리한다.

## IME와 isComposing

isComposing은 IME(Input Method Editor)에서 사용되는 이벤트의 속성이다.

IME는 사용자의 입력을 처리하고 문자를 조합하여 최종적으로 입력한 값은 생성하는 도구로, 한자나, 한글, 히라가나와 가타카나와 같은 일어 등에 입력에 많이 사용된다

isComposing은 사용자가 IME를 통하여 문자를 조합하는 중인지의 여부를 확인하는 속성이다.

## 해결방법

```typescript
function IMEInput() {
  const [value, setValue] = useState("");
  const [isComposing, setIsComposing] = useState(false);

  const handleComposition = (e: React.CompositionEvent<HTMLInputElement>) => {
    if (e.type === "compositionstart") {
      setIsComposing(true);
    }
    if (e.type === "compositionend") {
      setIsComposing(false);
    }
  };

  const handleChange = (event) => {
    if (!isComposing) {
      setValue(event.target.value);
    }
  };

  return (
    <input
      type="text"
      value={value}
      onChange={handleChange}
      onCompositionStart={handleComposition}
      onCompositionUpdate={handleComposition}
      onCompositionEnd={handleComposition}
    />
  );
}

export default IMEInput;
```

handleComposition() 메서드는 isComposing 상태를 확인하여 이벤트를 처리하는데, composition이 start, end 상태로 변할때 useState를 사용하여
isComposing의 상태를 업데이트 하여 오류를 해결하는 방식이다.
