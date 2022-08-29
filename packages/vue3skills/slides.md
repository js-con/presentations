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
  font-size: 1rem !important ;
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
layout: big-points
title: 副作用清除
titleRow: true
---

<div w="200">
<v-click>

useEventListener
```javascript
import { onUnmounted } from 'vue'

export function useEventListener(target: EventTarget, name:string, fn:any){
  target.addEventListener(name, fn)

  onUnmounted(()=>{
    target.removeEventListener(name, fn)
  })
}
```
</v-click>
</div>

<!-- 
在vue3中，其实它响应式底层的effect会在组件销毁时自动销毁。也就是说
你不用担心你watch了一个响应式变量，然后组件销毁了它还在watch，产生这样
一种副作用，也就是vue组件会自动做这件事情。
所以我们在编写自己的hooks函数时，也可以实现类似的这样一种模式。
（点击）比如我们这里一个useEventListener函数，作用是帮助注册监听事件，并且在
组件unmounted的时候清除事件。
同样的，在对上层API进行封装时，我们也可以遵循这样一种模式，降低用户的心智负担。
 -->

---
layout: big-points
title: 副作用清除
titleRow: true
---

<div w="200" flex="~">

<div m="r-4">

```javascript
function useDouble(counter: Ref<number>){
  const doubled = computed(()=> counter.value * 2)

  onUnmounted(()=>{
    stop(doubled.effect)
  })

  return {
    doubled
  }
}
```
</div>

<div>

```javascript
function useDouble(counter: Ref<number>){
  const disposables = []

  const doubled = computed(()=> counter.value * 2)
  disposables.push(()=>stop(doubled.effect))

  const stopWatch = watchEffect(()=>{
    console.log('counter changed:', counter.value)
  })
  disposables.push(stopWatch)

  // ...
  onUnmounted(()=>{
    disposables.forEach(f => f());
    disposables = []
  })

  // ...
}

```
</div>
</div>


<!-- 
那么有的同学可能注意到了，我们刚才说，在vue组件内部使用的effect，比如computed，watch等，
会在组件销毁时由vue统一销毁。
但是vue3中，这些响应式API可以独立于组件存在，比如我们编写了一个hooks函数，里面使用到了watch，
那这个销毁工作由谁来做呢？由于hooks其实就是javascript函数，所以vue肯定是管不到了。我们需要手动
来处理这件事情。
（点击）这里应该知道的是，vue为每个effect提供了一个stop函数，它用于清除副作用。比如在这个函数里，
我们在onUnmounted钩子中清除了内部创建的副作用。
但是这样就出现了一个痛点，如果一个hooks内部的effect过多，清理的代码会变得繁杂。在vue3刚出现时，
基本的清理思路就是这样
（点击）每创建一个effect时，就把它的清除函数塞入一个清除队列，最后在要释放的时候，遍历这个队列依次执行。
这样无疑是增加了代码的噪声，影响了整体可读性。
其实，vue3.2版本为我们提供了一个新的API来解决这个问题，但很多人还没有使用过（下一页）
 -->

---
layout: big-points
title: EffectScope
titleRow: true
---

<div w="200">
<div v-click>

```javascript
function effectScope(detached?: boolean): EffectScope

interface EffectScope {
  run<T>(fn: () => T): T | undefined
  stop(): void
}
```
</div>

<div v-click>

```javascript
const scope = effectScope()
scope.run(()=>{
  const doubled = computed(()=> counter.value * 2)

  watch(doubled, ()=> console.log(doubled.value))

  watchEffect(()=> console.log('Count:',doubled.value))

})

scope.stop()
```
</div>
</div>

<!-- 
这个API叫effectScope,它的签名大概是这样的（点击）
他的使用方式类似于一个构造函数，它提供一个run方法，用来运行一个effect，
相应的，它提供了一个stop方法，当调用stop时，其effect内部所有副作用都会被统一清除
这是一个使用例子（点击）
由于现在建立的新项目都是3.2版本了，大家不要忘记使用这个API
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
最后，我们来介绍一下vue3中的jsx的使用。有些同学可能知道，vue3其实对jsx也提供了更好的支持。（下一页）
那在咱们自己的项目中包括很多开源项目中，其实大家都更偏向于使用模板，
如果一个人更喜欢jsx，他干脆就会直接选择react。其实，vue中的jsx也非常灵活，同时在很多场景下
是要比模板表达性更强的。
 -->

---
layout: big-points
title: JSX 前世今生
titleRow: true
---

<div w="200" flex="~">

<div>
<ul>
<li>

### 最早由facebook起草，但不是tc39规范
</li>
<li>

### 浏览器不会实现
</li>
<li>

### 由编译器和框架自己实现
</li>
</ul>
</div>

<div>

```javascript
<h1>Hello, world!</h1>
```

```javascript
import { createVNode as _createVNode } from 'vue'

_createVNode('h1', null, "hello, world")
```
</div>
</div>

<!-- 
有些同学可能对JSX有些误解，他可能认为JSX是react的专利，其实并不是这样的
虽说是规范，但它和tc39是不一样的，因为它本质上是语法糖，它由各个框架或者工具链去进行
编译。
 -->

---
layout: big-points
title: vue3的JSX
titleRow: true
---

<div w="200" v-click>

### 插件：@vue/babel-plugin-jsx

```javascript
declare global {
  namespace JSX {
    interface Element {}
    interface ElementClass {
      $props: {}
    }
    interface ElementAttributeProperty{
      $props: {}
    }
    // ...
  }
}
```
</div>

<!-- 
如果有同学在vue2里写过jsx应该清楚，在vue2项目写jsx时需要装一些babel的包
它们用来编译和运行jsx。另外如果你想在vue2里写tsx你还额外需要装一个vue-tsx-support这个包
因为vue2源码本身不是用TS写的。
但是在vue3中，我们只用装一个@vue/babel-plugin-jsx就可以了
（点击）同时它还内置了jsx元素的类型，相当于开箱即用
 -->


---
layout: big-points
title: 什么时候使用JSX
titleRow: true
---

<div w="200" flex="~">

<div m="r-4" v-click>

```javascript

const List = defineComponent({
  setup(){
    return ()=>(<div>...</div>)
  }
})
const Footer = defineComponent({
  setup(){
    return ()=>(<div>...</div>)
  }
})
export default defineComponent({
  setup(){
    return ()=>(
      <>
        // ...
        <List />
        <Footer />
      </>
    )
  }
})
```
</div>
<div v-click>

```javascript
const renderContent = defineComponent({
  setup(props){
    const Content = [
      <div class="foo">Foo</div>,
      <div class="bar">Bar</div>
    ]
    if(props.shouldReverse)
      Content.reverse()

    return ()=>(
      <div>{ Content }</div>
    )
  }
})
```
</div>
</div>

<!-- 
模板和JSX各有优劣，在某些场景下，使用JSX编写vue3组件其实更加优雅
首先，第一个例子（点击）
很多情况下，将一个SFC拆分成一些细粒度的组件其实更有利于阅读，但是大部分人不愿意拆的原因
就是SFC是单文件组件，如果要拆就得拆到新文件中，而组件的粒度又挺细，就觉得拆了还不如不拆
这种时候，其实可以考虑一个文件里写多个组件，既保证了可读性，又不至于让代码过于分散。
比如说在这个文件中，我们的父组件包含了很多节点，现在我们想把它的List和Footer拆分出来，那么我们可以直接
在上面进行组件的定义，可以看到，这样的一个代码组织会更加清晰。其实不使用jsx也可以采取这种模式，但是
那种情况下我们需要写h函数，可能有些同学没有写过，刚才我们介绍了，在vue中，jsx会被编译成
createVNode函数，而h函数就是createVNode这个函数的一个简写，它们本质是一个函数。那h函数大家都知道,
写起来确实和jsx一样灵活，但是一旦节点多了，它的可读性就会非常差。
同时，我们知道，vue SFC是一个.vue后缀文件，它需要经过vue-compiler进行编译，那么我们在开发过程中的类型安全，拿vscode来说，其实是vue官方开发的插件volar来做的，这个插件的效果并没有TS那么好，尤其是组件
的层级关系比较复杂时，有些类型还是会丢失，而且它时长会占用比较大的内存。
但是，使用TSX进行开发时，由于TSX文件本质就是ts，可以享受到TS编译器的照顾，无需安装其他插件，类型
检查不再是个痛点。
(点击)第二个优点呢，就是使用jsx可以享受到javascript的完全编程能力，什么叫完全编程能力呢，其实就是
灵活性。我们可以自由的处置jsx元素，比如在这个例子中，我们直接把元素塞进一个数组，接着，如果组件参数中
传了shouldReverse,那么我们直接用js的数组方式操作节点即可。在类似这种情况下，其实它的可读性是要比模板
高一些的。
 -->


---
layout: big-points
title: JSX的Vue本地化
titleRow: true
---

<div w="200">

<div v-click>

```jsx
// v-show
<input v-show={this.visible.value} />
// v-if
{ visible.value && <input /> }
{ visible.value ? <input /> : <textarea />}

//v-bind
<input title={msg.value} />

// v-model
<input v-model={val} />
<input v-model={[val, "modifier"]}
<A v-models={[ [foo], [bar, "bar"] ]} />

// slots
const slots = {
  bar: () => <span>B</span>
}
<A v-slots={slots} />

// v-for
{ arr.map(i=><div key={i.id}>{i.name}</div>)}

```
</div>

</div>

<!-- 
下面，我们来讲一下Vue中的JSX和React中有什么不同。
大家都知道，vue的模板指令是一种魔法，需要经过编译，那在jsx中还能不能使用vue的内置指令呢？
其实是可以的（点击）
我们可以看到，比如v-show，可以直接使用，不过大家要注意，在模板中vue会自动帮你进行ref的解包
但是jsx就需要你自己手动去.value了。
接着是v-bind，我们不再需要，直接传入一个值即可。
然后是v-if也是没有的，因为借助JS本身的运算符就可以实现，比如这个短路运算符，或者是三元运算符
接着是v-model，在vue3中可以指定model名称，这里是以数组形式，第一个是值，第二个是名称
那么如果是多个v-model，就传入一个二维数组，注意这里是v-models，多了一个s
再是slot，以对象的形式传入，注意这里的值需要是一个函数
最后是v-for，和react里使用相同，直接用js的map去操作数组即可
其实在我一开始知道vue3的jsx原来这么厉害的时候，我也是比较吃惊的，感觉就像react里是手动挡，
vue3里是自动挡。确实，vue3的jsx提供了很多便利，但它同时也有一些坑需要注意,尤其是如果你是一个
react选手，那么要更加小心。
 -->

---
layout: big-points
title: 小心使用JSX
titleRow: true
---

<div w="200">

<ul>
<li v-click>

### vue3不能直接传递VNode给属性
</li>

<li v-click>

### vue3中不建议写纯函数组件
```javascript
function Button(props){
  return <button {...props}>按钮</button>
}
const Button = defineComponent({
  setup(props){
    return ()=><button {...props}>按钮</button>
  }
})
```
</li>

<li v-click>

### 大多数情况下JSX的性能不如模板
</li>
</ul>
</div>

<!-- 
（点击）第一个注意点就是vue组件的props不能直接传jsx节点，这和react不一样，
在react中我们可以使用children或者render props来传递jsx节点。
但是vue中要实现节点的传递只能用插槽。
（点击）第二个坑是vue3中最好不要写纯函数式组件。习惯react的用户可能会这么干，
但要知道，react组件render时，会同时render它的所有子组件，而vue则完全不同
写纯函数组件，你传入的props改变后，节点并不一定会更新，所以在vue3中尽量避免使用纯函数
式组件，使用defineComponent
（点击）第三个问题是在多数情况下，JSX的性能是不如模板好的，因为vue针对模板进行了很多优化，
使用JSX的话就享受不到。但这个差距仅在dom节点很多的时候才会比较明显，所以一般来说，
我们倾向于优先考虑代码可读性。
 -->

---
layout: big-points
title: 类型安全的provide/inject
titleRow: true
---

<div w="200" flex="~">
<div v-click m="r-4">

```javascript
const Child = defineComponent({
  setup(){
    const orderId = inject('orderId')
    return ()=><div>{orderId.value}</div>
  }
})
const Parent = defineComponent({
  setup(){
    const orderId = provide('orderId', orderId)
    return ()=><Child />
  }
})
```
</div>
<div v-click>

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
				id: 1iiiiiii,
				name: 'xxx'
			})
      // 子组件中 const user = inject(injectKeyUser)
	}
}
```
</div>
</div>

<!-- 
最后，再来分享一个很多人没用到的技巧。
刚才说到在项目中我们可以在一个文件中写多个组件，但是一旦组件比较多
props传来传去就比较麻烦，这时候我们最好使用vue3的provide/inject API
（点击）使用方法大家应该都知道，在父组件provide一个值，并指定一个字符串key，
同时在子组件inject这个key，就能拿到对应的值。
但是这里有个问题，这样写是不能保证类型安全的，这个key相当于是any，就是说
你这个inject获取的值可能不存在，一旦key写错了也就拿不到值了。
这里我们可以使用Vue提供的一个类型工具injectionKey来解决这个问题（点击）
我们首先把要provide的数据类型定义好，然后作为泛型参数传给injectionKey，
之后使用Symbol()来确保它全局唯一。
这样我们在inject时就能获得正确的类型提示了，也就保证了类型安全。
 -->

---
layout: outro
---

<!-- 
那么我最后总结一下，今天分享的内容虽然很简单，
但都是些很实用却容易被忽略的知识点，但如果每个人都能运用好
这些社区的优秀实践，提高自己代码的可读性，就能提高团队整体的
效率。虽然我们内部有些远古代码，一个组件上千行，已经难以进行重构，
但是我们可以从自己的新功能开始，在力所能及的范围内，提高代码的质量，
不仅是对项目负责，其实也间接提高了未来接手同学的体验和工作效率。
好的，今天的分享就到这里，谢谢大家的观看。
 -->

# 谢谢观看！
