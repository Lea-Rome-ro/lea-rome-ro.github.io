---
title: useMemo와 useCallback의 사용
date: "2021-07-10T18:40:32.169Z"
template: "post"
draft: false
slug: "useMemo-and-useCallback"
category: "React"
tags:
  - "React"
  - "useMemo"
  - "useCallback"
  - "Hook"
description: "useMemo와 useCallback의 개념에 대해 알아보고, 간단한 사용 방법에 대해서 알아볼 수 있습니다."
socialImage: "/media/React-icon.png"
---


## 1. useMemo란?

  성능 최적화를 위하여 연산된 값을 재사용하는 기능을 가진 함수로 컴포넌트가 리렌더링 될때마다 실행되는 불필요한 연산을 방지  

## 2. useMemo 사용 방법

 - ### useMemo를 사용하지 않은 경우

  ![no_useMemo](/media/no_useMemo0.jpg)

  ![no_useMemo](/media/no_useMemo.jpg)

  이렇게 변수를 정의하고, return 내에서 count값을 사용하게 되면 users에 변화가 없더라도 컴포넌트가 리렌더링 될때마다 countActiveUsers 함수가 실행되게 된다.

  이러한 상태에서 useMemo를 사용해 보자.

 - ### useMemo를 사용한 경우

  ![useMemo](/media/useMemo.jpg)
  
  이렇게 useMemo를 사용해서 count에 countActiveUsers함수를 할당하는 경우 최초 호출 시 연산한 값을 기억(memorized)해두었다가 내용이 바뀌지 않는다면 기억해두었던 원래의 연산값을 재사용하게 된다.

  **✔  useRef와의 차이**

  useRef는 단순히 특정 값을 기억한다면, useMemo는 deps가 변경되기 전까지의 값과 복잡한 함수의 return값을 보관하는 역할로 사용 


## 3. useCallback이란?

  useMemo가 특정 value를 재사용하기 위함이라면, useCallback은 특정 함수를 새로 만들지 않고 재사용하기 위해 사용하는 Hook
  
## 4. useCallback 사용 방법

 - ### useCallback으로 정의하고 조건을 달아준다

![useCallback](/media/useCallback.jpg)

  useMemo와 유사하게 각 함수마다 useCallback으로 정의하고, 조건을 달아준다. 함수 안에서 사용하는 상태 혹은 props가 있으면 꼭 deps배열 안에 포함시켜야 한다. 
  
  이렇게 useCallback을 사용하여 props가 바뀌지 않았으면 VirtualDOM에 새로 렌더링하지 않게 만들 수 있다.

## 5. react 리렌더링 줄이기

- useMemo / useCallback 사용
​- 단순히 태그 안에 {margin:10px;}와 같이 값읕 지정해 버리면 매번 다른 객체로 인시되어서 값이 바뀌지 않아도 해당 태그 부분이 리렌더링 될 수 있다. 따라서 스타일 지정 시 styled 사용해서 동일한 객체로 인식되어서 값이 바뀌었을때만 리렌더링이 일어나도록 적용해준다. 


- ####styled import 예시

![styled-component import](/media/styled.jpg)


## 추가 참고

https://crmrelease.tistory.com/41  
https://devrappers.tistory.com/60  
https://devrappers.tistory.com/61?category=856236  
https://react.vlpt.us/basic/18-useCallback.html​  