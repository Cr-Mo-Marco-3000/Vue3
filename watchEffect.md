# watchEffect

- watchEffet는 **반응형**(ref, reactive)으로 된 객체의 변화를 감지하여, 특정 함수를 실행시켜 준다.

- 컴포넌트가 mount 되기 전 한 번 실행되고, 이후 변화를 감지해서 실행된다.

- 형식은 다음과 같이 함수 형태로 쓰인다.
  
  - watchEffect(() => {실행할 내용})

- 골때리는 것은, watchEffect 안에 **직접 적힌** 정보가 변경되었을 때만, 변화를 감지할 수 있다는 것이다.

- 다음 코드를 보면서 이해하자

```js
/* 반응형 객체 안에 원시타입이 있는 경우 */
const tempString = ref('')

// tempString.value에 새로운 값을 넣었을 때
watchList(() => {
    // 반응하지 않는다 => tempString이라는 반응형 '객체'는 변화가 없기 때문이다!
    console.log(tempString)
    // 반응을 하게 하려면, 아래처럼 '직접' 변화하는 값을 적어주어야 한다.
    console.log(tempString.value)
})
```

```js
/* 반응형 객체 안에 객체(array 포함)이 있는 경우 */

const tempArray = ref([])

// tempArray에 새로운 값을 push로 추가했을 때
watchList(() => {
    // 반응하지 않는다 
    // => tempList이라는 반응형 '객체'는 변화가 없기 때문이다!
    console.log(tempList) 

    // 역시 반응하지 않는다. 
    // => 배열 내부의 값이 변경됐음에도, 참조하는 객체는 동일한 객체이기 때분이다.
    console.log(tempList.value)

    // 반응하게 해 주려면, 아래와 같이 변화하는 값을 직접 적어 주어야 한다!

    // => array의 마지막에 붙는 객체는 push될 때 마다 변화하므로 반응.
    console.log(tempList.value.at(-1)) 

    // => push될 때마다 길이가 변하므로 반응.
    console.log(tempList.value.length)
})
```
