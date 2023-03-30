---
layout: post
title: "[JS] Javascript는 싱글 스레드(Single Threaded)인데 어떻게 병렬처리(다중처리)가 가능할까?"
subtitle: "Javascript single threaded"
category: development
tags: client javascript threaded
image:
  path: /assets/img/development/client/2022-12-12/js_runtime_cover.png
---

<span style="font-size:30px;">\#**목차**</span>

* this unordered seed list will be replaced by the toc
{:toc}

# JavaScript의 Runtime환경
## Thread 란?

> “**Thread**는 브라우저가 사용자 이벤트를 처리하고 디스플레이를 렌더링하여 화면에 그려주고, 일반적인 웹 페이지 또는 앱을 구성하는 코드의 대부분을 실행하는 데 사용한다.” - [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Thread)

![js-engine](/assets/img/development/client/2022-12-12/js_engine.jpg){:.centered width="80%"}

> **Memory Heap(변수,객체 저장소)**: 객체와 변수를 할당하는 데 사용되는 비구조화 메모리의 한 부분.<br/>
**Call Stack(코드 실행)**: 실행 될 코드들이 메모리(Heap)에 할당된 변수 또는 객체를 찾아 호출되면서 코드가 실행되고 실행된 코드들은 Stack에서 제거.

- 즉, 위의 그림처럼 **Javascript Engine(Single Threaded)**에는 **`Memory Heap`**과 **`Call Stack`**으로 각 1개씩 구성되어 있다.


## Memory Heap 이란?

> “JavaScript는 객체가 생성될 때 자동으로 메모리를 할당하고 더 이상 사용되지 않을 때 메모리에서 제거한다([garbage collection](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)).<br> 이 자동성은 잠재적인 혼란의 원인된다. **왜냐하면, 개발자에게 메모리 관리에 대해 걱정할 필요가 없다는 잘못된 인상을 줄 수 있다.**” - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management#memory_life_cycle)

**Memory life cycle**
1. 필요한 메모리 할당
2. 할당된 메모리 사용(읽기, 쓰기)
3. 할당된 메모리가 더 이상 필요하지 않을 때 해제(제거)

**JavaScript에서의 할당**
- 값 초기화.
- 자바스크립트는 초기에 값이 선언될 때 자동으로 `Memory Heap`에 할당한다.

```javascript
var a = 10;                             // stack에 저장
let b = 'hello';                        // stack에 저장
const varPI = Math.PI;                  // stack에 저장
const singleThreadExample = () => {     // stack에 저장
  return b + " " + (varPI * a);
}
```

## Call Stack 이란?

> “**Call Stack**은 인터프리터(웹 브라우저의 자바스크립트 인터프리터)가 여러 함수를 호출하는 스크립트에서 자신의 위치를 추적하는 메커니즘.” - [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack)
- 스크립트가 함수를 호출하면 인터프리터는 함수를 호출 스택에 추가한 다음 함수를 수행하기 시작.
- `Heap`영역에 있는 해당 함수에 의해 호출되는 모든 함수는 스택에 추가되고 호출이 된 곳에서 기능을 실행.
- 현재 기능이 완료되면 인터프리터는 스택에서 해당 기능을 제거하고 마지막 코드 목록에서 종료한 위치에서 실행을 다시 시작.

```javascript
const multiply = (x, y) => {    
    return x * y;
}
function printSquare(x) {   
    let s = multiply(x, x);
    console.log(s);
}
printSquare(5); // Call Stack 영역

// 25
```

![call stack flow](/assets/img/development/client/2022-12-12/call_stack_flow.png){:.centered width="80%"}

#### Stack Overflow
- 스택이 할당된 공간보다 더 많은 공간을 차지하면 **"stack overflow"** 오류가 발생합니다.

```javascript
const foo = () => {
  foo();
}
foo();
```

![stack overflow](/assets/img/development/client/2022-12-12/stack_over_flow.png){:.centered width="80%"}

## WEB APIs 란?

![js-webapis](/assets/img/development/client/2022-12-12/js_webapis.jpg){:.centered width="80%"}

> “웹 API는 자바스크립트 엔진의 일부는 아니지만 브라우저가 제공하는 런타임 환경의 일부이다. 현대의 브라우저에는 매우 다양한 것을 가능하게 하는 수많은 API가 있다.”

- **Manipulate documents(문서 조작)**: 가장 일반적으로 사용되는 웹 API 중 하나는 개발자가 HTML과 CSS를 조작할 수 있게 해주는 DOM API로 HTML을 사용하면 HTML을 생성, 변경 및 제거하고 웹 페이지에 스타일을 동적으로 적용할 수 있다.
- **Draw and manipulate graphics(그래픽 그리기 및 조작)**: 브라우저에서 널리 지원되는 Canvas API 및 Web Graphics Library API를 사용하면 `<canvas>` 요소에 포함된 픽셀 데이터를 프로그래밍 방식으로 업데이트할 수 있다.
- **Fetch data from a server(서버에서 데이터 가져오기)**: Fetch API는 Request 및 Response 개체의 일반적인 정의를 사용하여 네트워크를 통해 리소스를 가져오는 인터페이스를 제공.

이벤트 수신기, 타이밍 함수 및 AJAX 요청과 같은 기능은 모두 작업이 트리거될 때까지 웹 API 컨테이너에 저장한다. 요청이 데이터 수신을 완료하거나 타이머가 설정된 시간에 도달하거나 클릭이 발생하면 콜백 함수가 콜백 대기열로 전송한다. - [WEB APIs](https://medium.com/@gemma.croad/understanding-the-javascript-runtime-environment-4dd8f52f6fca)


## Tasks(Callback) Queue 란?

![js-queue](/assets/img/development/client/2022-12-12/js_queue.jpg){:.centered width="80%"}

> “**Tasks(Callback) Queue**는 웹 API에서 전송된 콜백 함수를 추가된 순서대로 저장한다. 이 대기열은 선입선출로 실행되는 데이터 구조이다. 큐는 배열 푸시 방식을 사용하여 큐의 끝에 새로운 콜백 함수를 추가하고 배열 이동 방식을 사용하여 큐의 첫 번째 항목을 제거한다.”

콜백 함수는 콜 스택이 비워질 때까지 대기열에 저장된 다음 이벤트 루프에 의해 스택으로 이동이 된다.

## Event Loop 란?
> “**Event Loop**의 역할은 콜 스택과 콜백(Tasks Queue) 대기열의 상태를 지속적으로 모니터링하는 것이다. 스택이 비어 있으면 콜백 큐에서 콜백을 가져와 콜 스택에 배치하여 실행을 예약한다.”

# 결론!
![js_runtime](/assets/img/development/client/2022-12-12/js_runtime.jpg){:.centered width="80%"}

- 이것이 자바스크립트가 **단일 스레드** 언어임에도 불구하고 **비동기적으로 실행될 수 있는 것으로 묘사되는 이유**이다. 
- 자바스크립트는 한 번에 하나의 기능만 실행할 수 있기 때문에 동기화되지만 웹 API에서 콜백 큐로 콜백을 푸시 할 수 있고 이벤트 루프가 이러한 콜백을 지속적으로 콜 스택에 추가할 수 있기 때문에 자바스크립트는 비동기적으로 실행될 수 있다고 생각한다.

## 작동원리
1. JS Engine이 **`Heap`**영역에 변수를 저장해 놓는다.
2. JS Engine이 **`Stack`**영역에 작성한 코드들을 넣고 실행한다.
3. **`setTimeout`**, **`Ajax`**, **`async`**, **`Event Listener`**과 같은 **비동기** 작업들은 **`WEB APIs`**에 작업들을 넘긴다.
4. **`WEB APIs`**에서의 작업들은 **`Queue`**영역에와서 **`Stack`**영역이 비어있을때 **`Event Loop`**가 **`Stack`**영역으로 작업을 올려준다.

## 이해를 돕기위한 그림(Call Stack, WEB APIs)

![js-callstack](/assets/img/development/client/2022-12-12/js_callstack.gif){:.centered width="80%"}
<div style="text-align: center">[그림출처](https://beomy.github.io/tech/javascript/javascript-runtime/)</div>
**<center>Call Stack</center>**

<br><br>

![js_WEB_APIs](/assets/img/development/client/2022-12-12/js_WEB_APIs.gif){:.centered width="80%"}
<div style="text-align: center">[그림출처](https://beomy.github.io/tech/javascript/javascript-runtime/)</div>
**<div style="text-align: center;">WEB APIs</div>**

# 추가 자료
### 콜 스택(Call Stack)과 힙(Heap)
- **콜 스택(Call Stack)** : **원시타입 값**과 함수 호출의 **실행 컨텍스트(Execution Context)**를 저장하는 곳.
  - **원시타입이란?** : 객체가 아니면서 메서드도 가지지 않는 데이터 타입(`string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, `null`)
  - **실행 컨텍스트(Execution Context)란?** : 실행할 코드에 제공할 환경 정보들을 모아놓은 객체
    - 예제
```javascript
var temp = 'temp';
function b () { console.log('안녕하세용'); }
function a () { b(); }
a();
```
![Execution Context](/assets/img/development/client/2022-12-12/execution_context.png){:.centered width="80%"}

- **힙(Heap)** : 객체(`Object`), 배열(`Array`), 함수(`function`)와 같이 크키가 **동적**으로 변할 수 있는 **참조타입 값**을 저장하는 곳.

### 예제를 통한 동작 원리
```javascript
let a = 10;
let b = 35;
let arr = [];
function func() {
  const c = a + b;
  const obj = { d: c };
  return obj;
}
let o = func();
```

#### 1. GEC(Global Execution Context - 글로벌 실행 컨텍스트)가 생성되고 원시 값은 콜 스택에, 참조 값은 힙에 저장된다.
![Memory1](/assets/img/development/client/2022-12-12/memory1.png){:.centered width="80%"}

#### 2. 함수 func() 을 실행하게 되고 새로운 FEC(Function Execution Context)가 생성되며 동일하게 원시 값은 스택에, 참조 값은 힙에 저장된다.
![Memory2](/assets/img/development/client/2022-12-12/memory2.png){:.centered width="80%"}

#### 3. 이후, 함수 func() 이 객체 obj 를 리턴해서 o 에 저장된다. 리턴하기 때문에 FEC는 콜 스택에서 제거된다.
![Memory3](/assets/img/development/client/2022-12-12/memory3.png){:.centered width="80%"}

#### 4. 전체 코드가 실행이 끝나고 GEC가 콜 스택에서 제거된다. GEC가 제거됨에 따라서, 힙의 객체를 참조하는 스택의 값이 없기 때문에 가비지 컬렉터(Garbage Collector, GC)에 의해 제거된다.

# 추후 공부할 것
- [크롬브라우저로 확인하는 inner 방법](https://velog.io/@ggong/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8-execution-context){:target="_blank"}
- [[Javascript] Execution Context와 Lexical Environment](https://iamsjy17.github.io/javascript/2019/06/10/js33_execution_context.html){:target="_blank"}
- [JavaScript 식별자 찾기 대모험](https://homoefficio.github.io/2016/01/16/JavaScript-%EC%8B%9D%EB%B3%84%EC%9E%90-%EC%B0%BE%EA%B8%B0-%EB%8C%80%EB%AA%A8%ED%97%98/){:target="_blank"}

# Reference
- [콜 스택(Call stack)과 힙(Heap)](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/stack-heap.md){:target="_blank"}
- [JS Execution Context (실행 컨텍스트) 란?](https://gamguma.dev/post/2022/04/js_execution_context){:target="_blank"}
