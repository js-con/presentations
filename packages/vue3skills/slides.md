---
theme: vuetiful
clicks: 1
altCover: true
title: Vue3 实战技巧
fonts:
  mono: 'Comic Code Ligatures'
---

# Vue3 实战技巧
## 分享人：林博文


---

<Category />

---
layout: section
---

# 组合式API

---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<div m="b-4" w="200" text="xl" flex="~">

<div w="100" v-click>

```javascript
export default {
  data(){
		return{
			dark: false
		}
	},
	computed:{
		light(){
			return !this.dark
		}
	},
	methods:{
		toggleDark(){
			this.dark = !this.dark
		}
	}
}
```
</div>

<Ep1 m="l-10" v-click />

</div>

<!-- 
那么什么是组合式API呢？组合式API是vue3提供的一种全新的编写组件的方式。
我们先来回顾一下vue2中我们是如何编写组件的（点击）。大家都知道，在vue2中
我们是通过data,computed,methods等这些方法来构建vue组件上各种各样的功能。
那么这是一段给组件添加暗色模式的代码，（省略）
(点击)那么这张图就很好的展示出了在vue2组件中，逻辑的分布，我们可以看到，同样一个功能实现，分散在了不同的
方法中。
 -->

---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<div w="200" flex="~" text="xl">

<div m="r-4" v-click>

```html
<template>
  <div>
    <button @click="toggleDark">切换</button>
  </div>
</template>
<script>
import { ref, computed } from 'vue'
export default {
	setup() {
		const dark = ref(false)
		const light = computed(() => !dark.value)
		const toggleDark = ()=>{
			dark.value = !dark.value
		}
		return {
			dark,
			light,
			toggleDark
		}
	}
}
</script>
```
</div>


<div v-click>

```html
<script setup>
import { ref,computed } from 'vue'
const dark = ref(false)
const light = computed(() => !dark.value)
const toggleDark = ()=>{
  dark.value = !dark.value
}
</script>
```
</div>

</div>

<!-- 
接下来是我们的composition API，也就是组合式API
(介绍API)
那么这样看下来的话，其实好像感觉不到composition API的好处，
代码量也没有精简。
(介绍setup语法糖)
(点击)那其实我们使用script setup这种语法糖的形式的话，代码量会减少很多
所以从传统的方面来说的话，我们为什么要引入composition API呢？
要解答这个问题，我们需要更深入地了解composition API的特性，以及它与options API的区别
 -->

---
layout: big-points
title: 为什么要有组合式API?
titleRow: true
---

<div m="b-4" w="250" text="xl" flex="~" align="items-center">
<Ep2 v-click/>
<div v-click m="l-4">

```javascript
import { ref, computed } from '@vue/reactivity'
export function useDark(){
		const dark = ref(false)
		const light = computed(() => !dark.value)
		const toggleDark = ()=>{
			dark.value = !dark.value
		}
		return { dark,light,toggleDark }
}
```

```html
<template>
  <button @click="toggleDark">切换</button>
</template>
<script>
export default {
  setup(){
    const { dark,light,toggleDark } = useDark()
    return { dark,light,toggleDark }
  }
}
</script>
```
</div>

</div>

<!-- 
(点击)我们可以看到，options API和composition API的区别就像这张图所表现的一样，一个的逻辑分散在不同的组件方法中，另一个则
很自由，可以按逻辑进行组合,composition API之所以叫组合式API，是因为它可以让vue组件的功能像积木一样一层层的拼在一起
但有人可能会疑惑（代码量大一样不好维护）
那么接下来就要引出组合式API最强大的特性，那就是composition API的响应式系统是独立的，也就是说它可以不依赖vue3组件环境独立使用
这也是vue3区别于vue2的最大变化之一
（点击）我们还是以这个暗色模式为例，我们在另外一个文件,useDark.ts中直接定义这个函数（介绍）
接着，在组件中我们直接使用这个函数即可。（生命周期钩子也可以独立使用）（甚至可以函数套函数进行组合）
（举例Vueuse）
那么这样做的好处是什么呢？
假如这个组件有很多逻辑，且代码多到已经不利于维护，我们就可以用这种方法将逻辑拆分到各个文件中，极大的增强可维护性
 -->

---
layout: big-points
title: 总结
titleRow: true
---

<Ep3 />

<!-- 
下面我们总结一下，首先让我们看看options API的一些缺点
最直接的一个问题就是，options API不利于服用（点击）
在options API中，vue2也提供了一些手段方便开发者进行组件复用，比如说mixin，render，component
但是这些方法都存在着一些弊病，比方说（点击）潜在的命名冲突，还有（点击）上下文丢失，就是你不知道
代码里这个this里面的东西是从哪个mixin或者哪个component来的
但是在composition API中，它的复用其实是原生的javascript函数，没有其他限制
所以你可以像平常写javascript函数一样，随意的把组件里的逻辑抽出来变成新的函数
然后在多个不同的组件之间服用，灵活组合
（点击）接下来一点就是composition API提供了更好的Typescript支持,这也得益于它的灵活性
（点击）然后，相比于options API中我们按照API的类型来组织代码，在composition API中我们按照代码的逻辑进行组织
这样可以大大提高我们大型项目的可维护性。
（点击）再来一点就是composition API可以独立于Vue组件使用，就像刚才展示的那样
 -->

---
layout: section
---

# 优雅的自定义hooks

---
layout: big-points
title: ref 还是 reactive?
titleRow: true
---

<style>
li{
  padding: 0 !important;
  font-size: 0.8rem !important;
}
</style>

<div m="b-4" w="200" text="2xl" flex="~">
<div v-click="1" flex="1" m="r-4">
ref

```html
<script setup lang="ts">
import { ref } from 'vue'

let foo = 0
let bar = ref(0)

foo = 1
bar = 1 // ts-error
</script>
```

<div m="t-4" text="[1rem]">
  <div>
    <span>优点：</span>
    <ul>
      <li>显示调用，类型检查</li>
      <li>相比reactive限制更少</li>
    </ul>
  </div>

  <div m="t-2">
    <span>不足：</span>
    <ul>
      <li>.value</li>
    </ul>
  </div>
</div>
</div>

<div v-click="2" flex="1">
reactive

```html
<script setup lang="ts">
  import { reactive } from 'vue'

  const foo = { prop:0 }
  const bar = reactive({ prop: 0 })

  foo.prop = 1
  bar.prop = 1
</script>
```

<div m="t-4" text="[1rem]">
  <div>
    <span>优点：</span>
    <ul>
      <li>自动unwrap（不需要.value）</li>
    </ul>
  </div>

  <div m="t-2">
    <span>不足：</span>
    <ul>
      <li>在类型上和普通对象没区别</li>
      <li>使用ES6解构会丢失响应性</li>
      <li>watch需要使用箭头函数包装</li>
    </ul>
  </div>
</div>

</div>

</div>

<!-- 
那么接下来就开始分享我们实践中常用的一些技巧和模式
首先，大家最常遇到的一个问题就是，在composition API中，我们应该什么时候使用ref，什么时候又使用reactive呢？
这个问题没有标准答案，但是根据社区的经验（开源项目代码）以及vue团队成员antfu的推荐，就是能够使用ref的时候
尽量使用ref
（点击）首先第一个就是说，ref可以显示的使用.value进行使用，虽然很多人可能不喜欢这个.value对不对，但是这样使用的时候
我们自己就可以知道什么时候进行了响应式的数据更新
在这里可以看到foo是一个普通的number字面量，bar是一个ref变量，那么如果这个时候我不小心给bar赋值了一个number，就会得到一个
ts的编译错误提示，我缺了一个.value，我就可以避免一个运行时的错误；那么我写下.value的同时我也就可以知道在一些比如computed或者
watchEffect里面，我触发了一个数据的更新
(点击)那么，reactive的好处主要就是他可以自动unwrap，就是我不需要.value，但其实它有各种各样的限制
比如，很重要的一点就是，我们这里声明一个foo和一个bar，它们在类型上是完全一样的，就是说如果我不看上下文，我就不知道
我做的这个改动是reactive的;还有就是我们进行ES6的解构时，会导致它响应式的丢失，我们需要记得用toRef或者toRefs去包裹它；
然后就是我们用watch的话，一样的原因响应式丢失，所以我们需要用箭头函数去包裹它
 -->

---
layout: big-points
title: ref 还是 reactive?
titleRow: true
---

<div m="b-4" w="200" text="xl">

<div m="0" p="0" text="[0.5rem]">在众多的情况下，我们可以减少.value的使用</div>

<div>
<ul>
<li v-click>
`watch`直接接受Ref作为监听对象，并在回调函数中返回解包后的值

```javascript
const counter = ref(0)
watch(counter, count=>{
  console.log(count) // same as 'counter.value'
  })
```

</li>

<li v-click>
Ref在模板中自动解包
```html
  <template>
    <button @click="counter += 1">
      Counter is {{ counter }}
    </button>
  </template>
```

</li>

<li v-click>
使用reactive解包嵌套的Ref
```javascript
import { ref, reactive } from 'vue'
const foo = ref('bar')
const data = reactive({ foo, id: 10 })
data.foo // 'bar'
```
</li>
</ul>
</div>

</div>

<!-- 
虽然这里推荐大家使用ref，但确实很多人都不喜欢用.value,其实在很多情况下，vue会自动帮我们unwrap，这样我们减少
.value的使用
（点击）第一个就是在watch中，我们可以直接吧ref变量当作监听的对象,而在回调中它会自动解包，我们就不需要用.value
（点击）在一个就是在模板中使用ref对象不需要写.value，这个大家应该都知道,同时呢它赋值的时候你也不需要写.value
（点击）再一个就是如果你真的不希望写.value，或者你想统一对一个对象进行操作，你可以像这样把ref包裹在reactive中,
reactive返回出的属性是自动unwrap的。
这就是几个使用ref的技巧。
 -->

---
layout: big-points
title: ref的反操作 - unref
titleRow: true
---

<div m="b-4" w="200" text="xl">

<v-click>

- 实现:
```javascript
import { isRef } from 'vue'
function unref<T>( r: Ref<T> | T): T {
  return isRef(r) ? r.value : r
}
```
<br />
<br />
</v-click>

<v-click>

- 使用
```javascript
import { ref, unref } from 'vue'
const foo = ref('foo')
unref(foo) //foo
const bar = 'bar'
unref(bar) //bar
```
</v-click>
</div>

<!--
这里再介绍一个比较少见的api，unref
unref就是ref的反操作，也就是你传给他一个ref，他就给你ref的.value的值，如果是个普通值他就原样返回
它在源码里基本就是这样实现的（点击）
那么这么一个简单的工具有什么用呢，最大的用处就是（点击）
我们可以很无脑的把它套在所有的东西上，我可以把它套在所有的ref或者其他值上，我只用关心我要的值不是一个ref就行了
这是一个很少见但是很实用的技巧
 -->


---
layout: big-points
title: 接受ref作为参数
titleRow: true
---

<div m="t-8" w="250" text="xl" flex="col">

<div flex="~" align="items-start" v-click>
<div w="72" h="10" flex="~" align="items-center" justify="content-center" text="[1rem]">纯函数</div>
<div m="r-4" w="100">

```javascript
  function add(a: number, b:number){
    return a + b
  } 
```
</div>
<div>

```javascript
let a = 1
let b = 2
let c = add(a, b) //3
```
</div>
</div>

<div flex="~" align="items-start" w="100%" v-click>
<div w="72" h="20" flex="~" align="items-center" justify="content-center" text="[1rem]">接受Ref作为参数,返回一个响应式结果</div>
<div m="r-4" w="100">

```javascript
  function add(a: Ref<number>, b: Ref<number>){
    return computed(()=> a.value + b.value)
  } 
```
</div>
<div>

```javascript
const a = ref(1)
const b = ref(2)
const c = add(a, b)
c.value //3
```
</div>
</div>

<div flex="~" align="items-start" w="100%" v-click>
<div w="72" h="30" flex="~" align="items-center" justify="content-center" text="[1rem]" >同时接受传入值和Ref</div>
<div m="r-4" w="100">

```javascript
  function add(
    a: Ref<number> | number
    b: Ref<number> | number
  ){
    return computed(()=> unref(a) + unref(b))
  }  
```
</div>
<div>

```javascript
const a = ref(1)

const c = add(a, 5)
c.value //6

```
</div>
</div>

</div>

<!-- 
再了解unref之后，我们编写函数时有一个很实用的模式，就是接收ref作为函数的参数,我们以一个响应式的函数为例
（点击）这里是一个纯函数，（介绍），但是因为JS是一个procedure language，所以这里如果a跟b发生变化，c并不会跟着改变,
等于说它已经丢失了这一层连结的关系
（点击）那么我们可以把a和b改成Ref，那么c使用一个computed，这样不管a和b如何变化，c永远就等于a+b，是一个响应式的结果
那么其实可以看到说c也是一个ref，因为computed返回值也是Ref，那么如果你所有的函数都这样做的话，你在传参数的时候就不需要.value
包括你把c传给别的函数也是一样的
（点击）那么我们更进一步其实我们可以做的更灵活，比如这里我们a和b是一个union type也就是它可能是Ref或者一个number值,那么我们在
返回值里使用unref，也就是不管它是什么类型，我得到的了永远是它的值，这样我们可以说比如a是Ref，b是一个number，然后c永远是一个
a+5的状态。
通过这种模式，我们写出的函数更加简洁健壮，心智负担也比较小，不需要去在意.value之类的问题。
 -->

---
layout: big-points
title: 使用MaybeRef类型工具
titleRow: true
---

- 实现
```typescript
type = MaybeRef<T> = Ref<T> | T
```

<div m="b-4" w="200" text="xl">
<v-click>

- 使用前
```javascript
export function useTimeAgo(
	time: Date | number | string | Ref<Date | number | string>
){
	return computed( ()=> someFormating( unref(time) ) )
}
```
</v-click>
<v-click>

- 使用后
```javascript
type MaybeRef<T> = Ref<T> | T
export function useTimeAgo( MaybeRef<Date | number | string> ){
	return computed( ()=> someFormating( unref(time) ) )
}
```
</v-click>
</div>

<!-- 
下面再介绍一个简单的类型工具，就是可以自己定义的，给Typescript用的
在编写自定义hooks时，这个工具非常有用。vue hooks库 vueuse源码中就大量使用了这个工具
(点击)以useTimeAgo为例，我们可以接受非常多的类型，同时也希望接受Ref，那么这里可以看到我们需要写多余的代码，
是非常繁琐的
（点击）使用了MaybeRef后，代码就变得很简化，同时它也很好的兼容了unref
这是一个小技巧
 -->

---
layout: big-points
title: 重复使用已有ref
titleRow: true
---

<div m="b-4" w="200" text="xl">

<v-click>

- ### 常见多余代码
```javascript
const foo = ref(1)
const bar = isRef(foo) ? foo : ref(foo)
const bar = foo
```
<br />
<br />
</v-click>

<v-click>

- ### ref变量不会被构造函数重复构造
```javascript
const foo = ref(1)
const bar = ref(foo)
```
</v-click>
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
<v-click>

- ### 通常考虑
```javascript
const title = useTitle()
title.value = 'Hello World'
```

<br />
<br />
</v-click>
<v-click>

- ### 更加灵活
```javascript
const name = ref('Hello')
const title = computed( ()=> `${name.value} World` )
useTitle(title) //此时title和页面的title建立了连结
name.value = 'Hi' //页面标题变成 Hi World
```
</v-click>
</div>

---
layout: big-points
title: 状态共享
titleRow: true
---

<div m="b-4" w="200" text="xl">
<v-click>

- store.ts
```javascript
import { reactive } from 'vue'
export const store = reactive({
	state: {
			foo: 'bar'
	}
})
```
</v-click>

<v-click>

- A.vue
```javascript
import { store } from 'store'
store.state.foo = 'yeah'
```
</v-click>

<v-click>

- B.vue
```javascript
import { store } from 'store'
console.log(store.state.foo) // 'yeah'
```

</v-click>
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

---
layout: outro
---

# 谢谢观看！
