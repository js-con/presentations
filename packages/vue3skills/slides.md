---
theme: vuetiful
clicks: 1
altCover: true
title: Vue3 实战技巧
---

# Vue3 实战技巧


---

<Category />

---
layout: section
---

# 响应式API最佳实践

---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<Ep1 />

---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<div m="b-4" w="200" text="xl">

```html
<script>
  export default {
    setup(){
      const name = ref('')
      const welcomeMsg = computed(()=>`hello, ${name.value}`)
      function fetchName(){}
      onMounted(()=>{
        fetchName()
      })

      const board = ref([])
      function fetchBoard (){}
      onMounted(()=>{
        fetchBoard()
      })
      return {name, welcomeMsg, fetchName, board, fetchBoard}
    }
  }
</script>
```
</div>

---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<Ep2 />


---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<Ep3 />

---
layout: big-points
title: ref 还是 reactive?
titleRow: true
---

<div m="b-4" w="200" text="xl">

* 显示调用.value可触发TS类型检查
```html
<script setup lang="ts">
let foo = ref(1)
let bar = reactive(1)

let target = 0

target = foo //ts:error!
target = bar //nothing
</script>
```
</div>

---
layout: big-points
title: ref 还是 reactive?
titleRow: true
---

<div m="b-4" w="200" text="xl">

### reactive可以自动解包，但是有一些缺点：
- reactive在类型上和一般的对象没有区别
- 使用ES6解构时会失去响应性(toRef)
- 需要使用箭头函数包装才可以进行watch
```javascript
const foo = { prop: 0 }
const bar = reactive({
  prop: 0
})
//both OK
foo.prop = 1
bar.prop = 2
const { prop } = bar //lose reactivity
watch(()=>bar.prop, ()=>{})
```
</div>

---
layout: big-points
title: ref 还是 reactive?
titleRow: true
---

<div m="b-4" w="200" text="xl">

### ref在很多情况下也可以自动解包
- 模板中
- watch监听时
- 使用reactive包裹嵌套的ref
```html
<template>
  <div>{{ foo }}</div>
</template>
<script>
const foo = ref('bar')
const data = reactive({foo, id: 1})
data.foo = bar //ok
watch(foo,()=>{}) //ok
</script>
```
</div>

---
layout: big-points
title: ref的反操作 - unref
titleRow: true
---

<div m="b-4" w="200" text="xl">

- 实现:
```javascript
import { isRef } from 'vue'
function unref<T>( r: Ref<T> | T): T {
  return isRef(r) ? r.value : r
}
```
<br />
<br />

- 使用
```javascript
import { ref, unref } from 'vue'
const foo = ref('foo')
unref(foo) //foo
const bar = 'bar'
unref(bar) //bar
```
</div>

---
layout: big-points
title: ref的反操作 - unref
titleRow: true
---

<div m="b-4" w="200" text="xl">

- 接受ref作为参数
```javascript
import { Ref, computed, unref } from 'vue'

function add( a: Ref<number> | number, b: Ref<number> | number ){
	return computed( ()=>unref(a) + unref(b) )
}

const a = ref(1)
const c = add( a,5 )
console.log( c.value ) //6

a.value = 2
console.log( c.value ) //7
```
</div>

---
layout: big-points
title: ref的反操作 - unref
titleRow: true
---

### 使用MaybeRef类型工具

<div m="b-4" w="200" text="xl">

- 使用前
```javascript
export function useTimeAgo(
	time: Date | number | string | Ref<Date | number | string>
){
	return computed( ()=> someFormating( unref(time) ) )
}
```

- 使用后
```javascript
type MaybeRef<T> = Ref<T> | T
export function useTimeAgo( MaybeRef<Date | number | string> ){
	return computed( ()=> someFormating( unref(time) ) )
}
```
</div>

---

---
layout: section
---

# 优雅的自定义hooks

---
layout: big-points
title: 重复使用已有ref
titleRow: true
---

<div m="b-4" w="200" text="xl">

- 常见多余代码
```javascript
const foo = ref(1)
const bar = isRef(foo) ? foo : ref(foo)
const bar = foo
```
<br />
<br />

- ref变量不会被构造函数重复构造
```javascript
const foo = ref(1)
const bar = ref(foo)
```
</div>

---
layout: big-points
title: 使用由ref组成的对象
titleRow: true
---

<div m="b-4" w="200" text="xl">

```javascript
function useMouse() {
    // ...
    
    return {
        x: ref(0)
        y: ref(0)
    }
}

//直接使用
const { x } = useMouse()
x.value // 0

//其实可以自动解包（不需要.value)
const mouse = reactive(useMouse())

mouse.x // 0 
```
</div>

---
layout: big-points
title: 编写hook时尽量使其用法更灵活
titleRow: true
---

<div m="b-4" w="200" text="xl">

- 通常考虑
```javascript
const title = useTitle()
title.value = 'Hello World'
```

<br />
<br />

- 更加灵活
```javascript
const name = ref('Hello')
const title = computed( ()=> `${name.value} World` )
useTitle(title) //此时title和页面的title建立了连结
name.value = 'Hi' //页面标题变成 Hi World
```
</div>

---
layout: big-points
title: 状态共享
titleRow: true
---

<div m="b-4" w="200" text="xl">

- store.ts
```javascript
import { reactive } from 'vue'
export const store = reactive({
	state: {
			foo: 'bar'
	}
})
```

- A.vue
```javascript
import { store } from 'store'
store.state.foo = 'yeah'
```

- B.vue
```javascript
import { store } from 'store'
console.log(store.state.foo) // 'yeah'
```
</div>

---
layout: section
---

# 组件拆分细则

---
layout: big-points
title: 取舍
titleRow: true
---

<div m="b-4" text="xl">
一些从选项式 API 迁移来的用户发现，他们的组合式 API 代码缺乏组织性，并得出了组合式 API 在代码组织方面“更糟糕”的结论。我们建议持有这类观点的用户换个角度思考这个问题。
</div>

<div m="b-4" text="xl">
组合式 API 不像选项式 API 那样会手把手教你该把代码放在哪里。但反过来，它却让你可以像编写普通的 JavaScript 那样来编写组件代码。这意味着你能够，并且应该在写组合式 API 的代码时也运用上所有普通 JavaScript 代码组织的最佳实践。如果你可以编写组织良好的 JavaScript，你也应该有能力编写组织良好的组合式 API 代码。
</div>

<div m="b-4" text="xl">
选项式 API 确实允许你在编写组件代码时“少思考”，这是许多用户喜欢它的原因。然而，在减少费神思考的同时，它也将你锁定在规定的代码组织模式中，没有摆脱的余地，这会导致在更大规模的项目中难以进行重构或提高代码质量。在这方面，组合式 API 提供了更好的长期可维护性。 
</div>


---
layout: big-points
title: 逻辑关注点分离
titleRow: true
---

<div m="b-4" text="xl">
在实践中，我们在拆分组件hook时，不需要考虑拆分出来的hook是否能复用，而只应当关注它是否起到了"逻辑关注点分离"的作用。
</div>

<div m="b-4" text="xl">
这个原则和组件的拆分原则很像，最终目的都是使代码更加健壮，可维护性更强。
</div>

<div m="b-4" text="xl">
这里需要注意，拆分出来的文件通常以'useXXX'作为名称，而不再是vue2.x中的mixin，这样是为了将它们区分开来。
</div>


---
layout: big-points
title: 逻辑关注点分离
titleRow: true
---

<div m="b-4" w="200" text="xl">

  <div>
  假设我们有一个订单页面，其中包含了大部分订单的功能，我们需要把订单列表相关的数据抽离出来
  </div>

  <div>

  - 一种通用的目录结构
  <Ep4 />

  </div>


</div>

---
layout: big-points
title: 逻辑关注点分离
titleRow: true
---

<div m="b-4" w="200" text="xl">

```javascript
import { fetchOrderList } from '@/api'
export function useList(list: Ref<List>){
  const isError = ref(false)
  const isFinished = ref(false)
  const isLoading = ref(false)

  onMounted(()=>{
    fetchOrderList().then(({data, total})=>{
      list.value.push(...data)
      if(total === list.value.length){
        isFinished.value = true
      }
    }).catch(_=>{
      isError.value = true
    })
  }) 

  return {
    isError,isFinished,isLoading
  }
}
```
</div>


---
layout: big-points
title: 逻辑关注点分离
titleRow: true
---

<div m="b-4" w="200" text="xl">

```html
<template>
  <!-- ... -->>
  <div v-for="item in list" :key="item.id">
    {{item.xxx}} 
  </div>
  <Loading v-if="isLoading" />
  <Error v-if="isError" />
</template>
<script>
export default {
  setup(){
    // ...
    const list = ref<List>([])
    const { isError, isFinished, isLoading } = useList(list)
    return { list, isError, isFinished, isLoading }
  }
}
</script>
```
</div>

---
layout: section
---

# 其他技巧


---
layout: big-points
title: jsx in vue3
titleRow: true
---

<div m="b-4" w="200" text="xl">

- 插件：vuejs/babel-plugin-jsx
```javascript
export const Msg = defineComponent({
  setup(){
    const msg = ref('hello, world')
    return ()=>{
      <div>{ msg.value }</div>
    }
  }
})

export default{
  setup(){
    return ()=>{
      <div>
        <Msg />
      </div>
    }
  }
}
```
</div>

---
layout: big-points
title: 类型安全的组件参数
titleRow: true
---

<div m="b-4" w="200" text="xl">

- props:
```html
<script setup lang="ts">
const props = defineProps<{
  foo: string
  bar?: number
}>()

props.foo // string
props.bar // number | undefined
</script>
```

- emits
```html
<script setup lang="ts">
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
</script>
```
</div>

---
layout: big-points
title: 类型安全的provide/inject
titleRow: true
---

<div m="b-4" text="xl">
如果不主动包装类型，provide/inject将会变成any类型，非常不安全。
在vue3中，我们使用injectionKey来包装类型：
</div>

```javascript
interface UserInfo {
  id: number
  name: string
}
export const injectKeyUser: injectionKey<UserInfo> = Symbol()

import { provice } from 'vue'
import { injectKeyUser } from './context'
export default {
	setup(){
			provice( injectKeyUser, {
				id: 1,
				name: 'xxx'
			})
      // 子组件中 const user = inject(injectKeyUser)
	}
}
```
