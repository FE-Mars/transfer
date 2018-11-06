<template>
  <div class="pvui-transfer-panel">
    <div class="pvui-transfer-panel-title">
        <slot name="title">{{title}}</slot>
    </div>
    <div class="pvui-transfer-panel-main">
      <div class="pvui-transfer-panel-header">
        <el-input
          class="pvui-transfer-panel-search"
          :placeholder="placeholder"
          size="small"
          clearable>
        </el-input>
      </div>
      <div class="pvui-transfer-panel-body">
      
      </div>
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
  flex-direction: column;
  width: 200px;
  color: #181c25;
  .pvui-transfer-panel-title {
    text-align: left;
    line-height: 1;
    margin-bottom: 8px;
  }
  .pvui-transfer-panel-main {
    display: flex;
    height: 260px;
    border-radius: 3px;
    border: solid 1px #dddddd;
    background-color: #ffffff;
    box-sizing: border-box;
    .pvui-transfer-panel-header {
      padding: 10px 8px;
    }
    .pvui-transfer-panel-body {
      flex: 1;
    }
  }
}
</style>
