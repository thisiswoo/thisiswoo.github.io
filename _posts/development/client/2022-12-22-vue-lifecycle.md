---
layout: post
title: "[Vue] Vue.js 라이프사이클 이해하기"
subtitle: "Vue.js Lifecycle"
category: development
tags: client vue lifecycle
image:
  path: /assets/img/development/client/2022-12-22/lifecycle-hooks-in-vue3.png
---

<span style="font-size:30px;">\#**목차**</span>

* this unordered seed list will be replaced by the toc
{:toc}

## Vue Lifecycle 이란?

> “각 Vue 구성 요소 Instance는 생성될 때 일련의 초기화 단계를 거치게 된다. 예를 들어, 데이터 관찰(Create)을 설정하고 템플릿을 컴파일하고 DOM에 인스턴스를 마운트(Mount)하고 데이터가 변경될 때 DOM을 업데이트(Update)해야 한다. 그 과정에서 Lifecycle Hooks 이라는 기능도 실행하여 사용자가 특정 단계에서 자신의 코드를 추가할 수 있는 기회를 제공한다.” - [Vue Lifecycle](https://vuejs.org/guide/essentials/lifecycle.html)

## Vue Instance 란?

> “**Instance**란 모든 Vue.js 앱은 Vue를 사용하여 루트 Vue 인스턴스를 생성하여 부트스트랩된다.” - [Vue Instance](https://v1.vuejs.org/guide/instance.html)

즉, **Instance**는 Vue.js로 화면을 개발하기 위해 꼭 생성해야 하는 필수 단위.<br>
`new Vue({...})`로 선언하여 만들어진 객체를 Vue 인스턴스라고 부른다.
```vue
<script>
  new Vue({ // instance 생성
    // instance option properties
  });
</script>
```

## Composition API 란?

> “**컴포지션(Composition) API**는 컴포넌트 내에서 사용하는 **특정 기능을 갖는 코드를 유연하게 구성하여 사용할 수 있도록 Vue3 버전에 추가된 함수 기반의 API**이다. 컴포넌트 API가 생긴 이유는 Vue는 프로젝트 규모가 커질수록 관리하기 힘든 단점과 컴포넌트의 계층구조가 복잡할수록 코드에 대한 추적 및 관리가 힘든 점을 보안하기 위해 등장하게 되었다.” - [Composition API](https://sungjaecloud.tistory.com/346)

**`beforeCreate`** 및 **`created`**(**`setup()`** 메서드 자체로 대체됨)를 제외하고 설정 메소드에서 액세스할 수 있는 옵션 API lifecycle hooks 중 9개가 있다.

- **`onBeforeMount`** : mounting이 시작되기 전에 호출
- **`onMounted`** : 컴포넌트가 mount될 때 호출
- **`onBeforeUpdate`** : 반응형 데이터가 변경될 때와 re-render되기 전에 호출
- **`onUpdated`** : re-render된 후에 호출
- **`onBeforeUnmount`** : Vue 인스턴스가 파괴(삭제)되기 전에 호출
- **`onUnmounted`** : 인스턴스가 파괴(삭제)된 후 호출됨
- **`onActivated`** : 보관된 구성 요소가 활성화되면 호출
- **`onDeactivated`** : 보관된 구성 요소가 비활성화되면 호출
- **`onErrorCaptured`** : 하위 구성 요소에서 오류가 캡처되면 호출

## Vue 2 코드를 Vue 3 Lifecycle Hooks로 업데이트된 항목

- **beforeCreate** -> use **setup()**
- **created** -> use **setup()**
- **beforeMount** -> **onBeforeMount**
- **mounted** -> **onMounted**
- **beforeUpdate** -> **onBeforeUpdate**
- **updated** -> **onUpdated**
- **beforeDestroy** -> **onBeforeUnmount**
- **destroyed** -> **onUnmounted**
- **errorCaptured** -> **onErrorCaptured**

<!-- Continue with [MVVM 패턴이란?](adding-foldable-submenu-to-the-sidebar){:.heading.flip-title}
{:.read-more} -->

## Vue Lifecycle Hooks

![vue-lifecycle](/assets/img/development/client/2022-12-22/vue3_lifecycle_diagram.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://vuejs.org/guide/essentials/lifecycle.html)</div>

**<center>vue lifecycle hooks</center>**


## Creation Hooks

> “Composition API를 사용하는 Vue 3 Lifecycle Hooks의 경우 **`beforeCreate`**와 **`created`** 모두 **`setup()`** 메서드로 대체된다.” - [Creation Hooks](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

```javascript
import { ref } from 'vue';
export default {
  setup() {
    const val = ref('hello');
    console.log('Value of val is: ' + val.value); // Value of val is: hello
    return {
      val,
    }
  },
}
```

## Mounting Hooks
> “Mounting Hooks는 **`Component Mounting`** 및 **`Rendering`**을 처리한다.” - [Mounting Hooks](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

### **`beforeMount()`**와 **`onBeforeMount()`**
> “컴포넌트 DOM이 실제로 렌더링되고 **마운트되기 직전에 호출**된다. 이 단계에서는 `Root Element`가 아직 존재하지 않는다. Options API에서 `this.$el`을 사용하여 액세스할 수 있고, **Composition API**에선 `ref`를 사용해야 한다.” - [onBeforeMount()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)


**Options API**
```javascript
export default {
  beforeMount() {
    console.log(this.$el);  // null
  },
}
```

**Composition API**
```vue
<template>
  <div ref="root">Hello World</div>
</template>
<script>
import { ref, onBeforeMount } from 'vue';
export default {
  setup() {
    const root = ref(null);
    /* Composition API */
    onBeforeMount(() => {
      console.log(root.value); // null
    })
    return {
      root,
    }
  },
  beforeMount() {
    /* Options API */
    console.log(this.$el); // null
  },
}
</script>
```

### **`mount()`**와 **`onMount()`**
> “**Component의 첫 번째 렌더링 직후에 호출**된다. 이 단계에서는 직접 DOM 액세스를 허용하는 요소를 사용할 수 있다. Options API에서는 **`$el`**을 사용하여 DOM에 액세스하고, Composition API에서 Vue 라이프사이클 훅의 DOM에 액세스하려면 **`ref`**를 사용해야 한다.” - [onMount()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

```javascript
import { ref, onMounted } from 'vue';
export default {
  setup() {
    /* Composition API */
    const root = ref(null)
    onMounted(() => {
      console.log(root.value);  // null
    })
    return {
      root,
    }
  },
  mounted() {
    /* Options API */
    console.log(this.$el);  // <div ...>
  },
}
```

## Update Hooks
> “업데이트된 Lifecycle event는 반응형 데이터(Reactive Data)가 수정될 때마다 트리거되어 렌더 업데이트를 트리거된다.” - [Update Hooks](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

### **`beforeUpdate()`**와 **`onBeforeUpdate()`**
> “**데이터가 변경될 때 Component가 다시 렌더링되기 전에 실행**된다. 변경 사항이 발생하기 전에 DOM을 수동으로 업데이트하기에 좋은 곳이다.” - [onBeforeUpdate()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

### **`update()`**와 **`onUpdate()`**
> “**DOM이 업데이트되면 업데이트된 메서드가 호출**하게 된다.” - [onUpdate()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

```vue
<template>
  <div>
    <p>{{ val }} | edited {{ count }} times</p>
    <button @click="val = Math.random(0, 100)">Click to Change</button>
  </div>
</template>
```

**Options API**
```vue
<script>
export default {
  data() {
    return {
      val: 0,
    }
  },
  beforeUpdate() {
    console.log('beforeUpdate() val: ' + this.val); // beforeUpdate() val: 0.random value
  },
  updated() {
    console.log('updated() val: ' + this.val);  // updated() val: 0. change random value
  },
}
</script>
```

**Composition API**
```vue
<script>
import { ref, onBeforeUpdate, onUpdated } from 'vue';
export default {
  setup() {
    const count = ref(0);
    const val = ref(0);
    onBeforeUpdate(() => {
      count.value++
      console.log('beforeUpdate');  // beforeUpdate
    })
    onUpdated(() => {
      console.log('updated() val: ' + val.value); // updated() val: 0.random value
    })
    return {
      count,
      val,
    }
  },
}
</script>
```

## Destruction Hooks – Cleaning Things Up
> “Destruction Hooks는 **Component를 제거하고, 허술하게 남은 것들을 모두 치우는 과정에서 사용**된다. **제대로 처리되지 않으면 메모리 누수를 유발할 수 있는 이벤트 리스너와 항목을 제거**하게 된다.” - [Destruction Hooks](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

### **`beforeUnmounted()`**와 **`onBeforeUnmounted()`**
> “해당 단계는 Component가 분해되기 전이기 때문에, 전부는 아니더라도 대부분의 정리 작업을 수행해야 할 때이다. 이 단계에서 구성 요소는 여전히 완전히 작동하며 아직 손상된 것은 없다.” - [onBeforeUnmounted()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

**Options API**
```javascript
export default {
  mounted() {
    console.log('mount'); // mount
    window.addEventListener('resize', this.someMethod);
  },
  beforeUnmount() {
    console.log('unmount');
    window.removeEventListener('resize', this.someMethod);
  },
  methods: {
    someMethod() {
      // do something
    },
  },
}
```

**Composition API**
```javascript
import { onMounted, onBeforeUnmount } from 'vue';
export default {
  setup() {
    const someMethod = () => {
      // do smth
    }
    onMounted(() => {
      console.log('mount'); // mount
      window.addEventListener('resize', someMethod);
    })
    onBeforeUnmount(() => {
      console.log('unmount');
      window.removeEventListener('resize', someMethod);
    })
  },
}
```

* 코드가 업데이트되면 일부 구성 요소가 자체적으로 마운트 해제 및 마운트가 된다.

### **`unmounted()`**와 **`onUnmounted()`**
> “이 시점에서 대부분의 구성 요소와 구성 요소의 속성이 사라지므로 수행할 수 있는 작업이 많지 않다. **다시 한 번, 일부 데이터를 스캔하여 정확히 무엇이 남아 있는지, 프로젝트에 유용할 수 있는지 확인하는 단계**이다.” - [onBeforeUnmounted()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

```javascript
import { onUnmounted } from 'vue';
export default {
  setup() {
    /* Composition API */
    onUnmounted(() => {
      console.log('unmounted');
    })
  },
  unmounted() {
    /* Options API */
    console.log('unmounted');
  },
}
```

## Activation Hooks – Managing Keep-Alive Components
> “**`<keep-alive>`** 태그는 동적 Component의 **`wrapper element`** 이다. **동적 Component가 변경될 때마다 Vue가 완전히 새로운 인스턴스를 생성할 필요가 없도록 비활성 구성 요소에 대한 캐시된 참조를 저장**하게 된다.” - [Activation Hooks](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

### **`activated()`**와 **`onActivated()`**
> “이 메서드는 **활성 상태로 유지된 동적 구성 요소가 `"활성화"`될 때마다 호출**됩니다. 즉, 동적 Component의 active view가 됩니다.” - [onActivated()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

```vue
<template>
  <div>
    <span @click="tabName = 'Tab1'">Tab 1 </span>
    <span @click="tabName = 'Tab2'">Tab 2</span>
    <keep-alive>
      <component :is="tabName" class="tab-area" />
    </keep-alive>
  </div>
</template>
<script>
import Tab1 from './Tab1.vue';
import Tab2 from './Tab2.vue';
import { ref } from 'vue'
export default {
  components: {
    Tab1,
    Tab2,
  },
  setup() {
    /* Composition API */
    const tabName = ref('Tab1');
    return {
      tabName,
    }
  },
}
</script>
```

* `Tab1.vue` Component 내에서 활성화 후크에 다음과 같이 액세스할 수 있다.

```vue
<template>
  <div>
    <h2>Tab 1</h2>
    <input type="text" placeholder="this content will persist!" />
  </div>
</template>
<script>
import { onActivated } from 'vue';
export default {
  setup() {
    onActivated(() => {
      console.log('Tab 1 Activated');
    })
  },
}
</script>
```

### **`deactivated()`**와 **`onDeactivated()`**
> “활성 상태 유지 Component가 **더 이상 동적 구성 요소의 active view가 아닐 때 호출**된다. 이 Hooks는 특정 뷰가 포커스를 잃고 애니메이션을 트리거할 때 사용자 데이터를 저장하는 것과 같은 사용 사례에 유용할 수 있다.” - [onDeactivated()](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

```javascript
import { onActivated, onDeactivated } from 'vue';
export default {
  setup() {
    onActivated(() => {
      console.log('Tab 1 Activated');
    })
    onDeactivated(() => {
      console.log('Tab 1 Deactivated');
    })
  },
}
```

* 이제 탭을 전환하면 각 동적 구성 요소의 상태가 캐시되고 저장된다.

![vue-lifecycle](/assets/img/development/client/2022-12-22/activated-deactivated.gif){:.centered width="100%"}
<div style="text-align: center">[그림출처](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#deactivated-and-ondeactivated)</div>

**<center>Activated and Deactivated</center>**

## Vue 3 Debug Hooks
> “**Vue 3은 디버깅 목적**으로 사용할 수 있는 두 개의 후크를 제공한다.” - [Debug Hooks](https://learnvue.co/tutorials/vue-lifecycle-hooks-guide#what-about-the-composition-api-creation-hooks)

- **`onRenderTracked`**
- **`onRenderTriggered`**<br>
이 두 이벤트 모두 Vue 인스턴스에서 다시 렌더링을 일으키는 원인을 알려주는 **`DebuggerEvent`**를 사용한다.

```javascript
export default {
  onRenderTriggered(e) {
    debugger
    // inspect which dependency is causing the component to re-render
  },
}
```

# 결론!

- Vue 인스턴스는 크게 **`생성(create)`**되고, **`DOM에 부착(mount)`**되고, **`업데이트(update)`**되며, **`없어지는(destroy)`** 4가지 과정을 거치게 된다.
- 이 과정에서 Vue는 각각의 단계에서, Vue를 사용하는 사람들을 위해 훅(Hook)을 할 수 있도록 API를 제공하게 된다. 일반적으로 많이 사용하는 종류로는 **`beforeCreate`** 및 **`created`**(**`setup()`** 메서드 자체로 대체됨), **`beforeMount`**, **`mounted`**, **`beforeUpdate`**, **`updated`**, **`beforeDestroy`**, **`destroyed`**가 있다.

Back to [[Vue] Vue.js 란?](what-is-vuejs){:.heading.flip-title}
{:.read-more}