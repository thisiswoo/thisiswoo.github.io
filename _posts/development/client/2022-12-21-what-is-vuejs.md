---
layout: post
title: "[Vue] Vue.js 란?"
subtitle: "what is Vue.js"
category: development
tags: client vue
image:
  path: /assets/img/development/client/2022-12-21/what-is-vue-js.jpg
---
> Vue.js 란?

# 정의

* this unordered seed list will be replaced by the toc
{:toc}

## Vuejs 란?

> “**뷰(Vue)**는 사용자 인터페이스를 구축하기 위한 **자바스크립트 프레임워크**이다. 표준 HTML, CSS, 자바스크립트를 기반으로 구축되며 단순하든 복잡하든 사용자 인터페이스를 효율적으로 개발할 수 있도록 도와주는 선언적이고 구성요소 기반의 프로그래밍 모델을 제공.” - [Vuejs](https://vuejs.org/guide/introduction.html)

## 특징

> “**데이터 바인딩과 화면 단위를 컴포넌트 형태로 제공하며, 관련 API 를 지원하는데에 궁극적인 목적**”

- **MVVM 패턴**을 사용과 **Virtual DOM**의 사용
- Angular에서 지원하는 **양방향 데이터 바인딩(two-way binding)**을 동일하게 제공
- 컴포넌트 간 통신의 기본 골격은 React의 **단방향(one-way binding) 데이터** 흐름(부모 -> 자식)을 사용
- 다른 프런트엔드 프레임워크(Angular, React)와 비교했을 때 상대적으로 **가볍고 빠름**
- **문법이 단순하고 간결**하여 초기 학습 비용이 낮고 누구나 쉽게 접근 가능

## SPA(Single Page Application)

![SPA](/assets/img/development/client/2022-12-21/SPA.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://miro.medium.com/max/1400/1*8unTYz6pOhwryEb5b1S2Sg.png)</div>

**<center>SPA(Single Page Application)</center>**

> “**`SPA`는 서버에서 전체(HTML, CSS 및 JavaScript) 새 페이지를 로드하는 대신 현재 페이지를 동적으로 다시 작성하여 사용자와 지속적으로 상호 작용을 기반으로 추가 데이터를 검색하는 단일 페이지 웹 어플리케이션** 이다.” - [SPA](https://www.netsolutions.com/insights/single-page-application/)

<!-- Continue with [SPA 란?](adding-foldable-submenu-to-the-sidebar){:.heading.flip-title}
{:.read-more} -->

## SSR(Server Side Rendering)

![SSR](/assets/img/development/client/2022-12-21/SSR.jpg){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://miro.medium.com/max/1400/1*8unTYz6pOhwryEb5b1S2Sg.png)</div>

> “**`SSR`이란 서버에서 렌더링해서 클라이언트(브라우저)로 보낸 후 화면에 표시하는 기법**을 의미한다. 웹사이트의 JavaScript가 웹사이트의 서버에서 렌더링되면 완전히 렌더링된 페이지가 클라이언트로 전송되고 클라이언트의 JavaScript 번들이 참여하여 SPA 프레임워크가 작동하게된다.” - [SSR](https://www.heavy.ai/technical-glossary/server-side-rendering)

<!-- Continue with [SSR 란?](adding-foldable-submenu-to-the-sidebar){:.heading.flip-title}
{:.read-more} -->

## MVVM 패턴이란?

![MVVM](/assets/img/development/client/2022-12-21/mvvm.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://012.vuejs.org/guide/#View)</div>

**<center>MVVM pattern</center>**

> “Backend 로직과 Client 의 마크업 & 데이터 표현단을 분리하기 위한 구조로 전통적인 MVC 패턴의 방식에서 기인하였다. 간단하게 생각해서 화면 앞단의 회면 동작 관련 로직과 뒷단의 DB 데이터 처리 및 서버 로직을 분리하고, 뒷단에서 넘어온 데이터를 Model 에 담아 View 로 넘어주는 중간 지점이라고 보면 되겠다.” - [MVVM 패턴](https://joshua1988.github.io/web-development/client/vuejs/vuejs-tutorial-for-beginner/), [WIKI](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EB%B7%B0%EB%AA%A8%EB%8D%B8)

<!-- Continue with [MVVM 패턴이란?](adding-foldable-submenu-to-the-sidebar){:.heading.flip-title}
{:.read-more} -->

## Vue Insctance

> “**`new Vue({ ... })`**로 선언하여 만들어진 객체를 vue 인스턴스라고 부른다.”

- **`el`** : 태그에 지정한 id, 클래스명, 태그명으로 해당 태그와 vue 인스턴스를 연결하는 옵션.
- **`data`** : **`key`**와 **`value`**를 지정하는 **`json형식`**으로 데이터 입력 옵션.
- **`computed`** : **`getter/setter`**를 지정하는 옵션.

```javascript
var vm = new Vue({
  el: '#app',
  data: {
    message: 'hello instance'
  },
  computed: {
    ...
  }
});
```

![MVVM](/assets/img/development/client/2022-12-21/MVVM_pattern.jpeg){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://wikidocs.net/17701)</div>

**<center>Vuejs MVVM</center>**

<!-- Continue with [Vue Insctance 란?](adding-foldable-submenu-to-the-sidebar){:.heading.flip-title}
{:.read-more} -->

## 가상 DOM(Virtual DOM) 이란?

![Virtual DOM](/assets/img/development/client/2022-12-21/elm-runtime-virtual-dom.jpg){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://elmprogramming.com/virtual-dom.html)</div>

**<center>Virtual DOM</center>**

> “DOM(Document Object Model) 요소가 많아지면 자바스크립트로 DOM을 핸들링하는 일이 무거워진다. 그래서 DOM과 비슷한 구조로 자바스크립트를 만들게 된다. 이것은 Real DOM과는 달리 메모리에 올라가있는 것이기 때문에 비교적 매우 빠른 성능을 보인다. Vue js가 가상돔(Virtual DOM)을 수정하면 DOM을 수정하는 것보다 빠르다. vue는 가상돔(Virtual DOM)이 변경될 때마다 Real DOM과 비교해서 차이를 찾고 차이난 부분의 DOM만 수정하는 동작을 하게 된다.” - [Virtual DOM](https://medium.com/js-dojo/whats-new-in-vue-js-2-0-virtual-dom-dc4b5b827f40)

<!-- Continue with [Virtual DOM이란?](adding-foldable-submenu-to-the-sidebar){:.heading.flip-title}
{:.read-more} -->

## 컴포넌트(Component)

![Component](/assets/img/development/client/2022-12-21/components.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://vuejs.org/guide/essentials/component-basics.html)</div>

**<center>Component</center>**

> “화면에 비춰지는 뷰의 단위를 쪼개어 재활용이 가능한 형태로 관리하는 것이 컴포넌트이다. Vue에서 컴포넌트는 미리 정의된 옵션을 가진 Vue 인스턴스이다. 전역 등록과 지역 등록 존재한다.”

Vue는 **재사용이 가능한 컴포넌트**로 웹 페이지를 구성할 수 있다.<br/>
확장자가 `.vue`인 단일 파일에 HTML, Javascript, CSS 코드로 구성하여 사용한다.

![Component](/assets/img/development/client/2022-12-21/vue-component-structure.png){:.centered width="70%"}

## Vue Lifecycle

![vue-lifecycle](/assets/img/development/client/2022-12-21/how-to-use-lifecycle-hooks-in-vue3-1.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide)</div>

**<center>Vue Lifecycle</center>**

- Creation — 구성요소를 작성할 때 실행된다.
- Mounting — DOM이 마운트될 때 실행된다.
- Updates — 사후 대응 데이터가 수정될 때 실행된다.
- Destruction — 요소가 파괴되기 직전에 실행된다.

Continue with [[Vue] Vue.js 라이프사이클 이해하기](vue-lifecycle){:.heading.flip-title}
{:.read-more}

# 결론!

- **뷰(Vue)**는 사용자 인터페이스를 구축하기 위한 **자바스크립트 프레임워크**이다.
- **SPA**와 **SSR로 사용자에게 편의성을 제공**한다.
- **MVVM 패턴**을 사용과 **Virtual DOM**의 사용이 가능.
- **양방향 데이터 바인딩(two-way binding)**과 **단방향(one-way binding) 데이터 바인딩**이 가능하다.
- 다른 프런트엔드 프레임워크(Angular, React)와 비교했을 때 상대적으로 **가볍고 빠르다** 또한 **문법이 단순하고 간결**하여 다른 프레임워크 보다 비교적 배우기 수월하다.
