<template>
  <div flex="~" w="100%">
      <div m="r-[96px]">
        <div text="center">options API</div>
        <ul m="t-8" class="list">
            <li v-click="1">不利于复用</li>
            <li v-click="2">潜在命名冲突</li>
            <li v-click="3">上下文丢失</li>
            <li v-click="4">有限的类型支持</li>
            <li v-click="5">按API类型组织</li>
        </ul>
      </div>
      <div>
        <div text="center">composition API</div>
        <ul m="t-8" class="list">
            <li v-click="1">极易复用</li>
            <li v-click="2">可灵活组合</li>
            <li v-click="3">提供更好的上下文支持</li>
            <li v-click="4">更好的TS支持</li>
            <li v-click="5">按功能/逻辑组织</li>
            <li v-click="5">可独立于组件使用</li>
        </ul>
      </div>
  </div>
</template>


<script setup lang='ts'>

</script>


<style>
li{
  font-size: 1.23rem !important;
}
</style>
