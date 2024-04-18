# Vue3

이거 하는게 맞나...

<!-- vue3로 완전히 넘어오고 다시 봤는데 내가 놓치는것도 있었고 
그때는 이해못해서 날림으로 넘겼는데 다시 보니까 이해되는게 있더라 좀더 깊이있고 그래서 backend는 언제-->

# 핵심가이드 

## computed  # 일반적으로 readonly
### 계산된 캐싱 vs 메서드
https://v3-docs.vuejs-korea.org/guide/essentials/computed.html#computed-caching-vs-methods
`computed` 말하는건데 

요약은 매서드와 computed가 똑같은 역할을 하는데 무엇이 어떤 상황에 더 좋은가??
그러니까 내부 감지가 변수들이 반응하지 않는한 
여러곳에서 접근해도 getter이 반응을 안하고 이전에 계산된 캐싱값을 전달하니까 흠 완전히 이해함..? 

주로 매서드 방식은 `v-for`같은 리스트렌더링이나 computed사용하기 어려운 곳에 주로 사용한다 

![alt text](image.png)

## 렌더링

```typescript
<template v-if="ok">
  <h1>제목</h1>
  <p>단락 1</p>
  <p>단락 2</p>
</template>
```

엘리먼트 블록 필요할때 div로 안감싸도 괜찮다 
v-for도 같이 가상 템플릿으로 감싸기 가능 


```javascript
반복되는 DOM 컨텐츠가 단순하거나(컴포넌트도 없고, 상태를 가지는 DOM 앨리먼트도 없을때), 의도적으로 기본 리스트 렌더링 동작을 통해 성능 향샹을 꾀하는 경우가 아니라면, 가능한 한 언제나 v-for는 key 속성과 함께 사용하는 것을 권장합니다.
```
몰랐네... 쓰기 귀찮아서 key생략한 경우가 있었는데 

계산된 속성에서 reverse()와 sort() 사용에 주의하십시오! 이 두 가지 방법은 원본 배열을 수정하므로 계산된 속성의 getter 함수에서 피해야합니다. 다음 메서드를 호출하기 전에 원본 배열의 복사본을 만듭니다:

```diff
- return numbers.reverse()
+ return [...numbers].reverse()
```

## Form 입력 바인딩

### .lazy#

기본적으로 v-model은 각 input 이벤트 이후 데이터와 입력을 동기화합니다(위에 언급된 IME 구성 제외). 대신 change 이벤트 이후에 동기화하기 위해 .lazy 수식어를 추가할 수 있습니다.

```javascript
<!-- "input" 대신 "change" 이벤트 후에 동기화됨 -->
<input v-model.lazy="msg" />
```

### .number

사용자 입력이 자동으로 숫자로 유형 변환되도록 하려면, v-model 수식어로 .number를 추가하면 됩니다:

```javascript
<input v-model.number="age" />
```
값을 parseFloat()로 파싱할 수 없으면 원래 값이 대신 사용됩니다.

인풋에 type="number"가 있으면 .number 수식어가 자동으로 적용됩니다.

### .trim#
사용자 입력의 공백이 자동으로 트리밍되도록 하려면 v-model 수식어로 .trim을 추가하면 됩니다:

```javascript
<input v-model.trim="msg" />
```

와 몰랐다 ㅋㅋㅋ

## 감시자 

> api에서 할말 많네 
기본적으로 가이드에서는 이것만 알아가면 될듯 

```javascript
반응형 상태를 변경하면 Vue 컴포넌트 업데이트와 사용자가 만든 감시자 콜백이 모두 실행됩니다.

기본적으로 개발자가 생성한 감시자 콜백은 Vue 컴포넌트가 업데이트되기 전에 실행됩니다. 따라서 감시자 콜백 내에서 DOM에 접근하면 DOM이 Vue에 의해 업데이트되기 전의 상태입니다.

Vue에 의해 업데이트된 후 의 DOM을 감시자 콜백에서 접근하려면, flush: 'post' 옵션을 지정해야 합니다:
```

DOM보다 감시자가 먼저 업데이트 돼서 DOM이후 접근하려면 'post' 템플릿 ref같은거 사용할때 말하는거 같음 

> 노트할만한 핵심가이드는 끝임 




