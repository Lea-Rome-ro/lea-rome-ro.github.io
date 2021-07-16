---
title: VUE 성능 최적화
date: "2021-06-07T22:40:32.169Z"
template: "post"
draft: false
slug: "vue-performance"
category: "Vue"
tags:
  - "vue"
  - "Front-end"
description: "Vue 성능 최적화를 위한 방법을 설명합니다. 간단한 한 옵션을 통해 렌더링 성능을 개선할 수 있습니다."
socialImage: "/media/vue-icon.png"
---

뷰 성능을 개선하기 위해 어떤 방법들이 있을까 생각하던 중 잘 정리되어 있는 블로그가 있어서 소개해 드릴려고 합니다. 웹은 자바스크립트의 알고리즘보다 브라우저의 렌더링, 리플로우를 최대한 줄이는 것이 성능에 도움이 됩니다. 렌더링을 자주 하지 않는 컴포넌트에 대해서 v-once, keep-alive를 해준다던지 v-if와 v-show를 잘 구분해서 쓰는것도 사소하지만 성능에 도움이 됩니다.

# Vue 성능 개선하기

Vue 렌더링 성능 개선을 위한 간단한 방법 몇가지를 설명합니다.이 문서는 Vue.js 2.6.10 버전을 기준으로 작성되었으며, 자세한 설명은 참고 링크를 확인해주세요.

## 함수형 컴포넌트(Functional Component) 사용하기

함수형 컴포넌트는 Dumb(멍청한, 바보같은) 컴포넌트 또는 Stateless (상태 없는) 컴포넌트로 불리며, Data와 Lifecycle을 가지지 않은 컴포넌트를 지칭합니다.기존의 `props`만 가지고 있는 컴포넌트에 `functional` 선언을 통해 앱 성능을 향상시킬 수 있습니다.

### 함수형 컴포넌트 선언과 사용

```js
Vue.component("my-component", {
  functional: true,
  render: function (createElement, context) {
    // ...
  },
  // Props는 선택사항입니다.
  props: {
    // ...
  },
});
```

`2.5.0+` 이후로, 싱글 파일 컴포넌트를 사용하는 경우, 템플릿 기반의 함수형 컴포넌트를 정의할 수 있습니다.

```html
<template functional></template>
```

### [함수형 컴포넌트 성능 비교](https://codesandbox.io/s/vue-stateful-vs-functional-q38om)

## keep-alive로 동적 컴포넌트 캐시하기

동적으로 컴포넌트를 렌더링하면서 상태를 캐시해야하는 경우, `keep-alive`를 사용해 컴포넌트 상태를 캐시 할 수 있습니다. 컴포넌트는 destory 되지 않기 때문에 destroy lifecycle이 호출되지 않습니다.

- `2.1.0+` 이후로 `include`와 `exclude`로 조건부 캐시가 가능합니다.
- `2.2.0+` 이후로 컴포넌트가 활성화 또는 비활성화 되는 경우 `activated` 와 `deactivated` lifecycle이 호출됩니다.
- `2.5.0+` 이후로 `max`를 사용해 최대 컴포넌트 캐시 개수를 지정할 수 있습니다. 최대 개수가 넘어가면 가장 최근에 사용되어 캐시된 컴포넌트부터 파괴합니다.

```html
<!-- comma-delimited string -->
<keep-alive include="a,b"><component :is="view"></component></keep-alive
><!-- regex (use `v-bind`) -->
<keep-alive :include="/a|b/"><component :is="view"></component></keep-alive
><!-- Array (use `v-bind`) -->
<keep-alive :include="['a', 'b']"
  ><component :is="view"></component
></keep-alive>
```

### [`is`와 `keep-alive`를 이용한 동적 컴포넌트 사용하기](https://codesandbox.io/s/vue-template-fnh3r)

## v-once로 컴포넌트 컨텐츠 캐싱하기

첫 렌더링 이후 변경되지 않는 콘텐츠를 포함하는 경우, `v-once`를 사용해 업데이트 성능을 향상시킬수 있습니다.**`v-once`를 과도하게 사용하다보면 업데이트가 안되는 상황에서 디버깅이 어려울 수 있습니다.**

```html
<!-- single element -->
<span v-once>This will never change: {{msg}}</span
><!-- the element have children -->

<!-- component -->
<my-component v-once :comment="msg"></my-component>
```

> https://vuejs.org/v2/api/#v-oncehttps://vuejs.org/v2/guide/components-edge-cases.html#Cheap-Static-Components-with-v-once

## v-if와 v-show를 구분해서 사용하기

`v-if` 는 컴포넌트가 토글되는 상황에서 DOM이 제거되고 다시 만들어집니다. 또한 초기 렌더링에서 조건이 거짓인 경우 아무것도 하지 않습니다.`v-show`는 초기 조건과 관계 없이 렌더링되며, 실제로 CSS display 속성만을 변경합니다.초기 렌더링 시간이 오래 걸리더라도 지속적으로 토글되는 경우, `v-show`를 사용하는 것을 추천합니다.

```html
// v-if
<template v-if="display">
  <h1>Welcome</h1>
  <p>Hi there!</p>
</template>
// v-show
<h1 v-show="ok">Hello!</h1>
```

> https://kr.vuejs.org/v2/guide/conditional.html

## v-for에서 key를 사용해야 하는 이유

Vue에서 개별 DOM 노드들을 추적하고 기존 엘리먼트를 재사용, 재정렬하기 위해서 v-for의 각 항목들에 고유한 key 속성을 제공해야 합니다. key에 대한 이상적인 값은 각 항목을 식별할 수 있는 고유한 ID입니다. key는 Vue가 노드를 식별하는 일반적인 메커니즘이기 때문에 v-for와 특별히 연관되지 않는 다른 용도로도 사용됩니다.

**2.2.0 이상에서 v-for는 key 가 필수 입니다**

아래 예제에서는 동일한 배열을 렌더링하면서 배열에 삭제가 일어할 때, key를 index로 사용하면 발생하는 문제를 보여줍니다.Vue는 기존 vnode와 key를 비교하여 key가 삭제된 경우, 재정렬 방법에 대해서 알 수 없기 때문에 리스트를 전부 다시 그립니다. 아래 예제에서 `<transition-group>`은 해당 리스트의 삭제된 vnode를 리스트의 끝에 붙여서 삭제하는 형태로 transition을 수행합니다.

### [예제](https://codesandbox.io/s/object-constancy-dxxze)

> https://kr.vuejs.org/v2/guide/list.html#keyhttps://www.telerik.com/blogs/in-vue-when-do-i-actually-need-the-key-attribute-and-whyhttps://rimdev.io/the-v-for-key/

## computed와 watch를 구분해서 사용하기

프로그래밍 방식 두가지를 분류하면 `computed`는 선언형 방식이고, `watch`는 명령형 방식이라고 볼 수 있습니다. 종속된 값이 변경되는것을 예상하여 미리 계산하여 선언해놓고 사용하는 방식이 `computed` 이고, 특정 데이터를 감시하고 변경에 대한 반응이 (함수 호출과 같은) 필요한 경우 `watch`를 사용합니다.

### `computed`가 유용한 경우

- 데이터의 변화에 따라서 화면에 계산된 값을 화면에 출력해야 하는 경우
- 캐싱이 필요한 경우, 입력값이 같을 경우 기존 캐시된 데이터를 사용하기 때문에 computed 구현체가 호출되지 않습니다.

### `watch`를 사용해야 하는 경우

- 단순한 계산 외에 비동기 처리 로직이 포함되는 경우
- 캐싱없이 매번 호출해야하는 경우
- 템플릿에 출력하지 않는 값의 변화를 관찰해야 할 경우

### 참고

[https://rjs5730.github.io/livedata-blog/posts/vue/vue-performance](https://rjs5730.github.io/livedata-blog/posts/vue/vue-performance)

