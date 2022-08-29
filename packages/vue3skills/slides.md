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

<!-- 
因为有些同学可能没接触过vue3，所以首先我们来介绍一下组合式API
 -->

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

<!-- 
熟悉了组合式API之后，我们在项目中经常会拆分逻辑或者编写一些公共的hooks函数，
那么下面我就分享一些实用的技巧，可以让你的hooks更加的灵活
以下的技巧其实都不是我发明的，都是来源于社区的各种实践，以下将要分享的技巧都是
我认为对咱们日常的开发很有帮助的
 -->

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
在介绍技巧模式之前，先来解决一个大家最常遇到的一个问题，在composition API中，我们应该什么时候使用ref，什么时候又使用reactive呢？
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

<!-- 
就像积木一样，你可以让你的函数适应不同的场景
（点击）以useTitle这个hook为例，它会在内部构造一个特殊的Ref，它和文档的title关联起来
如果改变这个ref，就会相应的改变文档的title
这里有个问题，就是每次我需要使用这个hook时都需要创建一个Ref
其实我们可以把它设计的更加灵活（点击）
我们可以把它绑定到一个现有的Ref，这里有个title的Ref，我们把它传递给useTitle的时候，意味着把它和文档的title建立了
联系，同时这里对name赋值的时候，标题也会改变
-->

---
layout: big-points
title: useTitle实现
titleRow: true
---

<div w="200">

```javascript
import { ref, watch } from 'vue'
import { Mayberef } from '@vueuse/core'

export function useTitle(
  newTitle: MaybeRef<string | null | undefined>
)
{
  const title = ref( newTitle || document.title )

  watch(title, t=>{
    if(t != null)
      document.title = t
  }, { immediate: true })

  return title
}
```
</div>

<!-- 
（介绍）
这里用了一个技巧，我们可以看到传进来的newTitle不一定是ref，那么我们在内部又使用ref给它包裹了一层
这其实也是一个技巧, 叫重复使用已有的ref（下一页）
 -->

---
layout: big-points
title: 重复使用已有ref
titleRow: true
---

<div m="b-4" w="200" text="xl">

### ref变量不会被构造函数重复构造
<div m="y-8">

```javascript
const foo = ref(1) // Ref<1>
const bar = ref(foo) // Ref<1>

foo === bar // true
```
</div>

```javascript
function useFoo(foo: Ref<string> | string) {
  const bar = isRef(foo) ? foo : ref(foo)

  //其实可以直接
  const bar = ref(foo)
}
```

</div>


<!-- 
这其实是很多人不知道的ref的一个特点，这其实和unref很像
如果你将一个ref传给ref构造函数，它会原样返回
（点击）如果你不知道这个特点，你很可能会写出这样的代码
如果在hook之间互相嵌套过深的时候，你需要做的判断会非常地狱，所以这个技巧还是
非常实用的。
同时ref的这种设计也值得我们借鉴
-->

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

const { x } = useMouse()
const mouse = reactive(useMouse())

mouse.x === x.value // true
```
</div>

<!-- 
接下来是另外一种模式，在实践中我们能感觉到，很多时候返回一个由ref组成的对象会让函数
更加灵活。
比如这样一种情况（点击）useMouse这个函数会返回两个Ref变量，它们已和你鼠标位置的x，y坐标建立了联系
如果我想要使用x的时候，可以直接解构，由于x是Ref所以响应式不会丢失
如果我想做一个整体的响应式对象，或者我想使用mouse.x的形式访问坐标,甚至想将它整体传给其他hook
就可以使用reactive包裹hook的返回对象
这样我们就可以根据不同的情况去灵活的使用hook
 -->

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

<!-- 
最后一种模式是状态共享，大家可能都在网上看过很多文章，说vue3之后不需要vuex这种状态共享库了
其实本质原因还是composition API可以独立于组件使用
我们在实践中应该已经用过pinia这个状态库了（点击）
它本来是一个社区项目，后来被vue官方收编成为了vuex的正式替代。那么大家可以看到它的实现其实就是
一个hook
（点击*2）使用起来也非常简单，由于其内部变量都是响应式的，所以我们直接像使用普通对象一样去使用就好了。
同时在store中，我们可以使用computed watch等API完成更复杂的副作用，让store更加灵活。
-->

---
layout: section
---

# 组件拆分细则

<!-- 
那么介绍了这么多技巧，其实还有一个我们开发中的痛点没有涉及到，那就是究竟该怎么拆分组件的逻辑，何时应该拆分呢
这个问题虽然没有标准答案，但其实每个人刚开始开发vue3时都会被困扰到
 -->

---
layout: big-points
title: 取舍
titleRow: true
---

<v-click>

<div m="b-4" text="xl">
一些从选项式 API 迁移来的用户发现，他们的组合式 API 代码缺乏组织性，并得出了组合式 API 在代码组织方面“更糟糕”的结论。我们建议持有这类观点的用户换个角度思考这个问题。
</div>

<div m="b-4" text="xl">
组合式 API 不像选项式 API 那样会手把手教你该把代码放在哪里。但反过来，它却让你可以像编写普通的 JavaScript 那样来编写组件代码。这意味着你能够，并且应该在写组合式 API 的代码时也运用上所有普通 JavaScript 代码组织的最佳实践。如果你可以编写组织良好的 JavaScript，你也应该有能力编写组织良好的组合式 API 代码。
</div>

<div m="b-4" text="xl">
选项式 API 确实允许你在编写组件代码时“少思考”，这是许多用户喜欢它的原因。然而，在减少费神思考的同时，它也将你锁定在规定的代码组织模式中，没有摆脱的余地，这会导致在更大规模的项目中难以进行重构或提高代码质量。在这方面，组合式 API 提供了更好的长期可维护性。 
</div>

</v-click>

<!-- 
在实际的vue3项目中，大家应该都见过一些比较头疼的代码，最常见的就是在一个setup中堆了所有的功能，而且这个
组件还有上千行的长度。由于这种情况的普遍，也导致了社区里出现一种声音，那就是为什么我觉得vue3代码的可读性
反而不如vue2了呢？这个问题也被vue官方注意到，所以官方文档里专门写了一段回答（点击）
（读回答）
看起来好像懂了，但是好像又没懂。因为这个回答只说明了组合式API更加灵活，所以可以有更好的可维护性，但没有具体
地告诉你具体该怎么做。
由于组合式API可以独立于vue组件使用，它的灵活度和js的灵活度差不了太多。而且拆分逻辑这种事和你的熟练度也有一定关系，
所以其实这种具体的做法是没有一个标准答案的。
但是遵循一些原则，可以让你再拆分逻辑时更得心应手。（下一页）
 -->


---
layout: big-points
title: 逻辑关注点分离
titleRow: true
---

<v-click>

<div m="b-4" text="xl">
在实践中，我们在拆分组件hook时，不需要考虑拆分出来的hook是否能复用，而只应当关注它是否起到了"逻辑关注点分离"的作用。
</div>

<div m="b-4" text="xl">
这个原则和组件的拆分原则很像，最终目的都是使代码更加健壮，可维护性更强。
</div>

<div m="b-4" text="xl">
这里需要注意，拆分出来的文件通常以'useXXX'作为名称，而不再是vue2.x中的mixin，这样是为了将它们区分开来。
</div>

</v-click>

<!-- 
一些朋友在面临拆分逻辑的时候，他会认为，一些逻辑是没有公用性的，即使拆分出来也不能复用。那何必呢，
还是都写在setup里吧。这其实是有悖于组合式API设计的初衷的。
虽然我们上面说过，组合式API大大增强了代码的可复用性，但是是否需要复用却并不能成为你是否应该拆分逻辑的原因
因为组合式API设计的初衷是逻辑关注点分离（点击）（读）。也就是说，只要你想增强代码的可读性，可维护性，你就可以根据逻辑来拆分你的组件
至于什么时候拆分，以及是否需要拆分到不同的文件，这个就因人而异了。但是像刚才说的情况，上千行的组件，那肯定是
不对的。
下面我们分享一个实际场景中常见的例子。
 -->


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

<!-- 
（读）
下面是我们项目源码的目录，src下包含所有源码，而顶层的这个hooks文件夹用于存放通用的hooks函数
比如网络请求封装成useFetch，统一放在这个目录下
然后在不同的页面里，我们也可以建立页面级的hooks文件夹，假设这个订单页面，里面包含了很多内容，
组件比较复杂，这时候我们想把订单列表相关的逻辑抽出去，就可以创建一个useList文件，把这部分逻辑放进来
然后在组件里引用即可
 -->


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

<!-- 
那么具体的话，这里给出了一个详细的示例，在useList中，我们首先接受一个参数list，用来存放所有订单数据
我们定义了和列表有关的所有状态，比如判断是否加载出错isError,判断是否已经拉完了所有订单isFinished,
和加载动画的isLoading，然后我们使用onMounted生命周期函数，在里面进行订单的请求，根据请求结果设置
不同的状态，最后把状态都返回出去
注意，不光是ref computed watch这种API可以独立于组件使用，我们的所有生命周期钩子函数也可以独立于组件调用，
只不过只有函数在组件上下文被加载时，内部的生命周期函数才会运行。这也是hooks函数很强大的一点。
（下一页）
 -->


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

<!-- 
接下来，我们就可以直接在组件中解构出useList返回的各种参数，而我们再调用useList时，
它的生命周期也合并进了组件内，也就是在组件挂载的时候，它会去请求订单列表，并且赋值给list
这样就是一个完整的逻辑抽离的例子，以后如果组件出了bug，你看是和列表状态有关的，就可以直接找到
useList这个文件去进行修改。
想象一下如果这是options API，且没有进行逻辑抽离，而且功能又复杂，那么以后你修改的时候很可能
背上千行的代码埋没。
 -->

---
layout: section
---

# JSX in vue3

<!-- 
最后，我们来介绍一下vue3中的jsx的使用。大家可能都知道了，vue3对jsx也提供了更好的支持。（下一页）
 -->

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
