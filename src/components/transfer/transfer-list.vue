<template>
    <el-checkbox-group class="pvui-transfer-list" v-model="checked" @change="handleChange">
        <el-checkbox class="pvui-transfer-list-item" v-for="item in data" :label="item.key" :key="item.key">
          <checkbox-content :item="item"></checkbox-content>
        </el-checkbox>
    </el-checkbox-group>
</template>
<script>
export default {
  name: "TransferList",
  components: {
    checkboxContent: {
      inject: ['$transfer'],
      props: {
        item: Object
      },
      render(h) {
        return this.$transfer.$scopedSlots.default 
          ? this.$transfer.$scopedSlots.default({ item: this.item })
          : <span>{this.item.label}</span>;
      }
    }
  },
  props: {
    defaultChecked: {
      type: Array,
      default() {
        return [];
      }
    },
    value: Array,
    data: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    return {
      checked: []
    };
  },
  watch: {
    defaultChecked: {
      immediate: true,
      handler(val, oldVal) {
        this.checked = [...val];
      }
    }
  },
  methods: {
    handleChange() {
      this.$emit("change", this.checked);
    }
  }
};
</script>
<style lang="less">
.pvui-transfer-list {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  .pvui-transfer-list-item {
    margin-left: 0;
    height: 32px;
    line-height: 32px;
    font-size: 12px;
    .el-checkbox__label{
      font-size: 12px;
    }
  }
}
</style>
