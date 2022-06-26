# Template Syntax

- Vue는 컴포넌트 인스턴스의 데이터를 서술적으로 렌더링된 DOM에 바인딩할 수 있는 HTML 기반 템플릿 문법을 사용함.

- 모든 Vue 템플릿은 사양을 준수하는 브라우저 및 HTML 파서에서 문법을 분석할 수 있는 문법적으로 유효한 HTML

  

## Text Interpolation(텍스트 보간법)

- 데이터 바인딩의 가장 기본적인 형태
- `{{}}` 이중 중괄호 사용
  - `<span>메시지: {{ msg }}<span/>`
- 이중 중괄호 태그 내 msg는 해당 컴포넌트 인스턴스의 msg 속성의 값으로 대체됨.
- 또한, msg 속성이 변경될 때 마다 업데이트됨
- 데이터를 HTML이 아니라 일반 텍스트로 해석하기 때문에, HTML을 사용할 때는 v-html을 사용해야 한다.
  - 단, v-html 사용은 권장하지 않는다.



## Directive(디렉티브)

- **v-접두사가 있는 특수 속성**

- 속성 값은 단일 JS 표현식이 됨(v-for, v-on, v-slot은 예외)

- 표현식의 값이 변경될 때, 반응적으로 변경점을 DOM에 적용하는 역할을 한다.

- 인자(Arguments) - 공식 홈페이지명 인수
  - ':'(콜론)을 통해 전달인자를 받는 디렉티브가 존재한다.
  
- 수식어(Modifiers)
  - '.'(점)으로 표시되는 특수 접미사
  - directive를 특별한 방식으로 binding해야 함을 나타냄

  - 예시: `.prevent` 수식어는 트리거된 이벤트에서 `event.preventDefault()`를 호출하도록 `v-on` 디렉티브에 지시한다.

```vue
<form @submit.prevent="onSubmit">...</form>
```

- 더욱 자세한 내용들은 html 파일들 참조
  - 주의해야 할 부분만 본문에 표시함.



### 동적 인자

- v-on이나 v-bind에서 디렉티브의 인수를 대괄호로 감싸서 JS 표현식으로 사용 가능

#### - 주의!

- DOM 내 템플릿을 사용할 때, 브라우저가 속성 이름을 소문자로 강제로 변환한다.
- 따라서 :[attributeName]은 :[attributename]으로 강제 변환되므로, 
  컴포넌트에도 attributename로 속성을 정의해야 정상 동작한다.

```vue
<!-- attributeName의 평가된 값이, 예를 들어 href인 경우, 다음 두 줄은 같은 코드이다.-->

<a :[attributeName]='url'>뀨잉뀨잉</a>
<a v-bind:href='url'>뀨잉뀨잉</a>

```





### v-html

- 쓰지 말 것
  - XSS 공격에 취약할 수 있다.



### v-bind

- HTML 엘리먼트의 속성(attribute)을 컴포넌트의 속성과 동기화된 상태로 유지하게 함

- 전용 단축 문법 ':'

  

### v-on







## JS 표현식 사용

- 데이터 바인딩 내 JS **표현식**의 모든 기능 제공
  - `const a = 1` 등 선언식이나 if 문 같은 흐름 제어는 작동하지 않음

- Vue 템플릿에서, JS 표현식은 다음과 같은 위치에 사용 가능

1. 이중 중괄호 내부

```vue
{{ number + 1 }}  

{{ ok ? '예' : '아니오' }}

{{ message.split('').reverse().join('') }}
```

2. 모든 Vue 디렉티브 속성(v-로 시작하는 특수 속성) 내부

```vue
<div :id="`list-${id}`"></div>
```



- 함수도 호출 가능
  - 단, 컴포넌트가 업데이트 될 때마다 호출되므로 주의해야 한다.

```vue
<span :title="toTitleDate(date)">
  {{ formatDate(date) }}
</span>
```

