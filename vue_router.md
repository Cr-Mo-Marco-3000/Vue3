# Vue Router

- Vue에서 Single Page Application을 만들 때 사용하는 library로, html을 불러오는 요청을 새로 보내지 않고 url을 통해 vue component 간 이동을 가능하게 한다.

## 사용법

- vue-cli 상에서 `vue add vue-router` 하는 방법도 있지만, 버전 문제가 생길 가능성도 높다.

- `npm install vue-router`을 한 뒤, vue project 상의 main.js에서 다음 코드를 붙인다.

```js
import router from './router'

createApp(App).use(router)
```

## 일반적인 기능

- 일반적으로, 다음과 같이 사용한다.

```js
// vue router를 설치하면 자동으로 입력되는 코드
import { createRouter, createWebHistory } from 'vue-router'

// routes에 매칭할 views 페이지들을 import 해준다.
import StartView from '../views/StartView.vue'
import MainPage from '@/views/MainPage.vue'
import ErrorPage from '@/views/ErrorPage.vue'

// navigation guard에서 사용하기 위해, vuex의 store 폴더에서 import를 해 준다
import store from '@/store'

// path에는 url이, name에는 편한 활용을 위한 지칭어가, component는 불러올 view파일이 들어간다.
const routes = [
  {
    path: '/',
    name: 'startpage',
    component: StartView,
  },
  {
    path: '/lounge',
    name: 'mainpage',
    component: MainPage,
  },
  // variable routing을 위해, :이후 변수를 입력해준다.
  {
    path: '/error/:errorname',
    name: 'errorpage',
    component: ErrorPage,
  },
  {
      // vue2에서는 '*'로 path를 지정하면 위에서 걸리지 않은 모든 페이지를 redirect해 주었지만
    // vue3에서는 다소 복잡한 아래 path를 입력해야 한다.
    path: '/:pathMatch(.*)*',
    redirect: '/error/404',
  },
]

// router를 import해서 사용하기 위해 선언해준다.
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
})


// router를 import해서 사용하기 위해 export 해 준다.
export default router 
```

## Navigation Guard

- 특정 변수의 상태에 따라, 특정 페이지로의 접근을 제한하는 Navigation Guard를 설정할 수 있다.
  
  ```js
  router.beforeEach((to, from, next) => {
  
  // store에서 로그인이 되었는지 확인하는 getters를 불러온다.
  const { isLoggedin } = store.getters
  
  // to.name은, 위에서 const route에서 지정한 view page들의 이름이다.
  // 본 예시 사이트에서는 startpage에 회원가입과 로그인 기능이 컴포넌트로 들어가 있으므로, 로그인 되어 있는 경우 그 페이지로의 접근을 차단하고, 로그인이 되어 있지 않은 경우 그 이외의 사이트로의 접근을 차단한다.
  
  // 스타트페이지가 아닌 곳으로 접근하고, 로그인이 되어 있지 않을 때,
  if (to.name !== 'startpage' && !isLoggedin) {
    alert('로그인이 필요한 페이지입니다')
  // next는, redirect와 비슷한 개념이다.
    next({ name: 'startpage' })
  } else {
    next()
  }
  // 로그인이 되어 있는데 스타트페이지로 접근할 때
  if (to.name === 'startpage' && isLoggedin) {
    next({ name: 'mainpage' })
  }
  })
  ```


