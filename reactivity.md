# 반응형

- Vue 3에서는, API 스타일로 두 가지 종류를 지원한다.
- 하나는 옵션 API이고 하나는 컴포지션 API인데, Vue3에서 새로 나온 컴포지션 API는 보다 큰 규모의 프로젝트에 적합하다.
- 옵션 API는 기존에 했던 Vue2와 크게 다를 바 없으니, 이번에는 컴포지션 API로 반응형 페이지를 구축하도록 한다.



## Composition API

- composition api를 사용하는 방법은 두 가지가 있다.

1. 기존과 같이 script 안에 export default 객체를 만든 후 내부에 setup() 훅을 작성하기
2. script 뒤에 setup을 붙인 후, setup() 훅 안에서 하는 것 처럼 작성해주기
   - 2번 방법을 사용할 경우, name과 inheritAttrs, customOptions들의 API option을 적용하지 못하는 문제점이 있다.



### setup()

- 내부에 반응형 객체, 배열을 위한 reactive(), ref()
- DOM 조작을 위한 함수들을 넣는 methods, computed, watch
- 기존 라이프 사이클 훅 mount의 setup 내부 버전 onmount 등이 들어간다.



### reactive()

- `import { reactive } from 'vue'`로 불러와서 사용한다.

- reactive() 함수를 사용하여 **객체 또는 배열을** 반응형으로 만들 수 있다.
- 또는 Map, Set과 같은 컬렉션 유형도 반응형으로 만들 수 있다.
- 그러나 string, number, boolean과 같은 원시 자료형은 **반응형으로 만들 수 없다!**
- Vue의 반응형 변경 감지는 속성에 접근함으로써 작동함으로, 항상 반응형 객체에 동일한 참조를 유지해야 한다. 즉, 연결 손실 때문에 반응형 객체를 교체할 수 없다는 것이다.
- 반응형 객체의 속성을 지역 변수에 할당하거나 구조 분해 할당, 또는 함수에 전달할 경우, 반응형 연결이 끊어진다!

```js
import { reactive } from 'vue'  

const state = reactive({ count: 0 })

// n은 지역 변수에 재할당되었으므로 반응형 연결이 끊어진다.
let n = state.count

// 원본인(state.count)의 상태에 영향을 미치지 않는다.
n++
  
// 지역 변수 count는 state.count와 연결이 끊어진다.
let { count } = state

// 역시 원본에 영향을 미치지 않는다.
count++

```



### ref()

- `import { ref } from 'vue'`로 불러와서 사용한다.

- **어떠한 유형의 데이터**라도 반응형으로 재정의할 수 있다.
- ref()는 받은 인수를 .value 속성을 포함하는 ref 객체에 래핑 후 반환한다.
  - 따라서 내부값에 접근할 때는, `.value`로 접근해야 한다.

```js
import { ref } from 'vue'

const count = ref(0)

console.log(count) // { value: 0 } => 객체로래핑되어 나온다.
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

- 반응형 상태로 함수에 전달되거나 구조 분해 할당되어도, 연결이 끊어지지 않는다.
- 즉 ref()를 사용하면 모든 값에 대한 "참조"를 만들어 반응성을 잃지 않고 전달할 수 있다.
  - 즉, ref 함수는 내부의 특정 value를 가리키고 있으므로, value 안의 값이 바뀌어도 항상 그 value를 가리키는 것은 변하지 않기 때문에 value의 값을 바꿀 수 있다는 것이다.



```js
const obj = {
  foo: ref(1),
  bar: ref(2)
}

// 함수가 ref를 전달받는다.
// .value를 통해 값에 접근해야 하지만
// 반응형 연결 상태가 유지된다.
functionName(obj.foo)

// 구조 분해 할당해도, 연결이 유지된다.
const { foo, bar } = obj

// ref(), reactive()의 특성 구체적인 예시

// 각각 반응형 선언
const appInput = ref('맛있겠다') 
let appInput2 = reactive('맛있겠네')

// 다른 변수의 재할당
const text = appInput
const text2 = appInput2

text.value = '옴뇸뇸뇸'
appInput2 = '옴뇸뇸뇸'

console.log(appInput.value) // 옴뇸뇸뇸

console.log(appInput2)
console.log(text2)
```



#### 응용

##### 템플릿에서 ref 언래핑

- 최상위 속성의 ref를 템플릿에서 접근하면 자동으로 "언래핑"되므로 .value를 통해 값에 접근할 필요가 없다.

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }} <!-- .value가 필요하지 않습니다. -->
  </button>
</template>
```

- 단, 이러한 템플릿에서의 언래핑은 ref객체가 최상위 속성인 경우에만 적용된다.

```js
// template에서 {{ object.foo }}로 접근할 수 없다.
const object = { foo: ref(1) }

// {{ object.foo.value }}로 접근하거나
// 이럴 때는 구조분해할당을 이용해서, foo를 최상위 속성으로 만들어서 해결한다.
// 아래 코드는 {{ foo }}로 접근 가능하다.
const foo = object

```



##### 반응형 객체에서 ref 언래핑

- `ref()`가 반응형 객체의 속성으로 접근하거나 변경되면 자동으로 언래핑되어 일반 속성처럼 작동한다.

```js
// ref '객체' count
const count = ref(0)

// reactive 반응형 객체 안에 객체를 넣었다.
const state = reactive({ count })

// 근데 count의 객체가 벗겨져서 아래와 같이 접근 가능하다.
console.log(state.count) // 0

// state.count의 값을 바꾸면
state.count = 1

// count.value로 접근하는 ref 객체의 값도 바뀐다
console.log(count.value) // 1
```



##### 배열 및 컬렉션에서 ref 언래핑

- 반응형 객체와 달리 ref를 반응형 배열의 요소로서 접근하거나 Map과 같은 기본 컬렉션 유형에서 접근할 때 언래핑이 실행되지 않는다.

