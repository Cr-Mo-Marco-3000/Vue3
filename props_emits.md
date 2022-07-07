# Props and Emits

- export default에서는 emits, props를 만든 후 내부에 선언
  - setup의 첫 번째 인자로 props, 두 번째 인자로 context를 받아준다.
  - 사용할 때는 context.emit의 형태로 쓴다.
- `<script setup>`에서는 `import { defineProps, defineEmits } from 'vue'` 해준 후 사용할 필요는 없다(문법상으로는). 그런데 eslint가 최신 버전이 아니면 필요하다고 인식한다.

- emit으로 보낸 데이터를 처리할 때는, 반응형으로 만든 데이터를 수정하는 식으로 사용한다.