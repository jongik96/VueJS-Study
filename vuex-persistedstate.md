
## vue-persistedstate 를 왜 사용하는가?!
vuex를 사용하는 프로젝트는 프로젝트 전체에서 사용되는 변수를 store의 state에 저장을 하고 사용한다.
vuex-persistedstate는 이 state에 저장된 변수와 값을 그대로 웹브라우저의 localstorage에 업데이트를 해준다.
(state와 localstorage를 동기화 해준다.)
localstorage에 등록된 내용은 쿠키나 세션처럼 화면을 새로고침해도 없어지지 않기 때문에 새로 화면이 로딩을 하게되면
localstorage에 있는 내용을 state에 다시 동기화를 시켜주게 된다.
결과적으로 새로고침을 해도 값들이 다시 그대로 존재하게 되는 것

## 고려해야할 점
state의 변수를 너무 많이 사용하게 되면 프로그램의 퍼포먼스가 떨어지는 단점이 있기 때문에 
vuex-persistedstate의 옵션으로 state 중 저장이 필요한 변수만 선택하여 저장하는 것이 좋다.

## vuex-persistedstate 설치
- npm 방식
  npm install --save vuex-persistedstate
- yarn 방식
  yarn add vuex-persistedstate

## 기본설정
- store의 root인 index.js에 한번만 적용하면 된다.
```javascript
import createPersistedState from "vuex-persistedstate";
const store = new Vuex.Store({
  // ...
  plugins: [createPersistedState()],
});
```

## 선택한 모듈만 설정
```javascript
import Vue from "vue";
import Vuex from "vuex";
import createPersistedState from "vuex-persistedstate";
import { Auth } from "./auth";
import { Cart } from "./cart";
import { Products } from "./products";
Vue.use(Vuex);
export default new Vuex.Store({
  modules: {
    user: User
  },
  plugins:[
    createPersistedState({
    paths: ["user"]   // 여기에 모듈을 쓰면 적용된다.
    }),
   ],
});
```

### 팀플젝하다가 알게된 플러그인.. 역시 프로젝트하면서 많이 배우는 것 같다
