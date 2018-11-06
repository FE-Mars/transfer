<template>
  <div class="pvui-transfer-panel">
    <div class="pvui-transfer-panel-title">
        <slot name="title">{{title}}</slot>
    </div>
    <div class="pvui-transfer-panel-main">
    
    </div>
  </div>
</template>
<script>
export default {
  name: "TransferPanel",
  props: {
    title: String,
    placeholder: String
  }
};
</script>
<style lang="less">
.pvui-transfer-panel {
  display: flex;
}
</style>
