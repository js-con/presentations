<template>
  <div class="ep3">
    <v-clicks>
      <div>
        <div>options API</div>
        <div>composition API</div>
      </div>
      <div>
        <li>不利于复用</li>
        <li>极易服用</li>
      </div>
      <div>
        <li>潜在命名冲突</li>
        <li>可灵活组合</li>
      </div>
      <div>
        <li>上下文丢失</li>
        <li>提供更好的上下文支持</li>
      </div>
      <div>
        <li>有限的类型支持</li>
        <li>更好的类型支持</li>
      </div>
      <div>
        <li>按API类型组织</li>
        <li>按功能逻辑组织</li>
      </div>
      <div>
        <li>只能在vue组件中使用</li>
        <li>可独立于vue组件使用</li>
      </div>
    </v-clicks>
  </div>
</template>


<script setup lang='ts'>

</script>


<style>
.ep3{
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
.ep3 div{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.ep3 div div{
  width: 200px;
  margin-right: 72px;
  margin-bottom: 16px;
}
.ep3 div li{
  width: 200px;
  margin-right: 72px;
}
</style>
