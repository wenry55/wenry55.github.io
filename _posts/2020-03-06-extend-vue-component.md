
다음과 같은 component design pattern을 먼저 살펴보도록 합시다.

* Props-driven template logic
* Slots
* JavaScript utility functions


##### Props-driven template logic

이 패턴은 컴포넌트에 넘어오는 파라메터에 따라서 template이 달라지는 형태이다.

##### MyVersatileComponent.vue
```js
<template>
  <div class="wrapper">
    <div v-if="type === 'a'">...</div>
    <div v-else-if="type === 'b'">...</div>
    <!--etc etc-->
  </div>
</template>
<script>
export default {
  props: { type: String },
  ...
}
</script>
```

##### ParentComponent.vue
```js
<template>
  <MyVersatileComponent type="a" />
  <MyVersatileComponent type="b" />
</template>
```

Here are two indicators that you’ve either hit the limits of this pattern or are misusing it:
The component composition model makes an app scalable by breaking down the state and logic into atomic parts. If you have too many variations in the one component (a “mega component”) you’ll find it won’t be readable or maintainable.
Props and template logic are intended to make a component dynamic but come at a runtime resource cost. It’s an anti-pattern if you’re using this mechanism to solve a code composition problem at runtime.

#### Slots

템플릿내에서 컴포넌트를 명시할 때 컴포넌트 태그안의 내용들은 모두 slot content 가 됩니다. 그 중에서 name이 지정되지 않은 것은 "default"라는 이름의 슬롯에 표현됩니다.

슬롯이라는 것이 사실은 컴포넌트이기도 합니다. 
```
<my-component>
  
</my-component>
```