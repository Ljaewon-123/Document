# Vuex?  X 이거 안씀 Pinia

vue.js 앱에서 사용 가능한 `상태 관리 라이브러리`

모든 컴포넌트에 대한 중앙 집중식 저장소 역할을 함 

vuex에 대해 굉장히 잘 정리된 블로그

```
https://www.bottlehs.com/vue/vue-js-vuex-state-mutations-actions-getters-%EC%9D%B4%ED%95%B4-%EB%B0%8F-%ED%99%9C%EC%9A%A9/
```

# store?

vuex에서 상태관리 하기위해 공유된 상태를 공유하는 전역변수

```typescript
//src/store/index.ts

import Vue from 'vue'
import Vuex from 'vex'

Vue.use(Vuex)


const store = new Vuex.Store({
    state: {

    },
    mutations: {

    },
    getters:{

    },
    actions:{

    }
})

export default store
```

흔히 이런 모습을 띄우는데 크게 

4가지로 구분할수 있다.

- sate

- mutations

- getters

- actions

다른 컴포넌트에 ts부분에서 import해서 사용가능 `하긴함`

요즘 pinia 덕에 일반 store는 export안하기도 하는듯

store를 두고 

stores라는 이름으로 pinia를 2개다 사용하기도 함

## state

여러 컴포넌트에서 공유할 공동의 상태

```typescript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        count: 1
    }
})
```

```html
<template>
  <div v-if="$store.state.count">
  </div>
</template>
같은경우로 템플릿에서 사용가능 
methos 에서 사용하면 앞에 this. 를 붙여야함 사용가능한지?
```

## mutations

mutations는 **state를 변경시키기 위한 방법**이다. 또한 **동기적 로직**을 정의할 수 있다.

methods 에서 사용 **getters와 다르게 인자(payload)를 받아서 넘겨줄수있음**

```typescript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        count: 1
    },
    mutations: {
        doubleCount(state) {
            state.count = state.count * 2
        },
        multiCountAndNumber(state, payload) {
            state.count = state.count * payload
        }
    }
})
```

```typescript
<script lang="ts">
export default {
  data: function () {
    return {
      number: 0,
    };
  },
  methods: {
    doubleCount() {
      this.$store.commit("doubleCount"); // state 실
    },
    multiInputNumber() {
      this.$store.commit("multiCountAndNumber", this.number);
    },
  },
};
</script>
```

`$store.commit('mutations에있는 이름',매개변수)`

## getters

공동의 상태를 계산하여 state의 값을 변환하는 함수 computed와 유사

몇몇 예제들이 다 computed에서만 사용한다 

**state에 있는 변수들을 computed에서 속성?으로 가공해줄수있는 놈**

**즉 computed에서 store.state에있는 변수를 가공해야할때**

```typescript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        alphabet: " ABC "
    },
    getters: {
        getLowerCaseAlphabet(state) {
            return state.alphabet.toLowerCase();
        },
        getTrimAlphabet(state) {
            return state.alphabet.trim();
        }
    },

})
```

```typescript
<script lnag="ts">

  computed: {
    getLower() {
      return this.$store.getters.getLowerCaseAlphabet;
    },
    getTrim() {
      return this.$store.getters.getTrimAlphabet;
    },
  },

</script>
```

## actions

mutations를 비동기 처리 

# Pinia

```url
https://pinia.vuejs.org/core-concepts/
```

`mutations` 없이 `actions`단계에서 전부처리 

Composition API 처럼 처리할수있게 해줌 

```typescript
import { defineStore } from "pinia";
```

`defineStore ` 일단이거에 대한 기본만 잡자면 

```typescript
import { defineStore } from 'pinia'

// You can name the return value of `defineStore()` anything you want, but it's best to use the name of the store and surround it with `use` and `Store` (e.g. `useUserStore`, `useCartStore`, `useProductStore`)
// the first argument is a unique id of the store across your application
export const useStore = defineStore('main', {
  // other options...
})
```

이렇게 `defineStore`의 첫번째 인수로는 고유한 이름을 주어야 하고 안에 각 속성들을 정의할수있다 

기존 **Vuex**와 같이 `import`해서 사용하거나 template에서도 사용가능하다.

pinia는 () => {} 화살표 함수 안써짐
