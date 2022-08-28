<template>
  <div flex="~" w="100%">
    <div m="r-[96px]">
      <div text="center">options API</div>
      <ul m="t-8" class="list">
        <li>不利于复用</li>
        <li>潜在命名冲突</li>
        <li>上下文丢失</li>
        <li>有限的类型支持</li>
        <li>按API类型组织</li>
      </ul>
    </div>
    <div>
      <div text="center">composition API</div>
      <ul m="t-8" class="list">
        <li>极易复用</li>
        <li>可灵活组合</li>
        <li>提供更好的上下文支持</li>
        <li>更好的TS支持</li>
        <li>按功能/逻辑组织</li>
        <li>可独立于组件使用</li>
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
