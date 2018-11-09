<template>
  <div class="pvui-transfer">
    <transfer-panel 
      v-bind="$props"
      :title="titles[0]" 
      :groupable="groupable"
      :data="sourceData" 
      :placeholder="filterPlaceholder"
      :default-checked="leftChecked"
      @change="handleLeftChange">
      <template slot="title">
        <slot name="left-title"></slot>
      </template>
    </transfer-panel>
    <div class="pvui-transfer-btns">
      <el-button type="primary" icon="el-icon-arrow-right" size="small" :disabled="!leftChecked.length" @click="addValues"></el-button>
      <el-button type="primary" icon="el-icon-arrow-left" size="small" :disabled="!rightChecked.length" @click="removeValues"></el-button>
    </div>
    <transfer-panel 
      v-bind="$props" 
      :groupable="false" 
      :title="titles[1]" 
      :data="targetData"
      :placeholder="filterPlaceholder"
      :default-checked="rightChecked"
      @change="handleRightChange">
      <template slot="title">
        <slot name="right-title"></slot>
      </template>
    </transfer-panel>
  </div>
</template>
<script>
import TransferPanel from "./transfer-panel.vue";
export default {
  name: "Transfer",
  provide() {
    return {
      $transfer: this
    };
  },
  components: {
    TransferPanel
  },
  props: {
    titles: {         //标题
      type: Array,
      default() {
        return [];
      }
    },
    selectAllable: Boolean,
    selectAllTexts: Array,
    format: {
      type: Object,
      default() { return {}; }
    },
    filterable: Boolean,    //是否启用搜索
    filterPlaceholder: String,     //搜索框占位符
    filterMethod: Function,     //自定义搜索方法
    groupable: Boolean,     //左侧列表是否启用分组    仅在groupTitles存在时生效   
    beforeLeftChange: Function,     //value改变前的方法
    beforeRightChange: Function,     //value改变前的方法
    data: {                 //数据源       
      type: Array,
      default() {
        return [];
      }
    },
    props: {            //数据源的字段别名
      type: Object,
      default() {
        return {
          title: 'title',
          items: 'items',
          label: "label",
          key: "key"
        };
      }
    },
    buttonTexts: {        //自定义按钮文案
      type: Array,
      default() {
        return [];
      }
    },
    leftDefaultChecked: {       //初始状态下左侧列表的已勾选项的 key 数组
      type: Array,
      default() {
        return [];
      }
    },
    rightDefaultChecked: {      //初始状态下右侧列表的已勾选项的 key 数组
      type: Array,
      default() {
        return [];
      }
    },
    value: {          //默认值
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    function getCustomProp(props) {
      let obj = {};
      Object.keys(props).forEach(key => {
        obj[`${key}Prop`] = props[key];
      })
      return obj;
    }
    return {
      leftChecked: [],
      rightChecked: [],
       ...getCustomProp(this.props)
    };
  },
  computed: {
    sourceData() {
      return this.data.map(group => ({
        [this.titleProp]: group[this.titleProp] || '',
        [this.itemsProp]: group[this.itemsProp].filter(item => !this.value.includes(item[this.keyProp]))
      }))
    },
    targetData() {
      return this.data.map(group => ({
        [this.titleProp]: group[this.props.title] || '',
        [this.itemsProp]: group[this.itemsProp].filter(item => this.value.includes(item[this.keyProp]))
      }))
    }
  },
  watch: {
    leftDefaultChecked: {
      immediate: true,
      handler(val, oldVal) {
        this.leftChecked = [...val];
      }
    },
    rightDefaultChecked: {
      immediate: true,
      handler(val, oldVal) {
        this.rightChecked = [...val];
      }
    }
  },
  methods: {
    emitModel(value) {
      this.$emit('input', value);
      this.$emit('change', value);
    },
    handleLeftChange(checked) {
      this.leftChecked = checked;
    },
    handleRightChange(checked) {
      this.rightChecked = checked;
    },
    addValues() {
      if(typeof this.beforeRightChange === 'function' && !this.beforeRightChange(this.value, this.leftChecked)) return;

      let currentValues = [...this.value, ...this.leftChecked];
      this.leftChecked = [];
      this.emitModel(currentValues);
      
    },
    removeValues() {
      if(typeof this.beforeLeftChange === 'function' && !this.beforeLeftChange(this.value, this.rightChecked)) return;

      let currentValues = this.value.filter(val => !this.rightChecked.includes(val));
      this.rightChecked = [];
      this.emitModel(currentValues);
    }
  }
};
</script>
<style lang="less" scoped>
.pvui-transfer {
  display: flex;
  font-size: 12px;

  .pvui-transfer-btns{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin: 0 14px;
    .el-button{
      padding: 6px;
      font-size: 14px;
      &+.el-button{
        margin-left: 0;
        margin-top: 8px;
      }
      &.is-disabled{
        color: #91959e;
        background-color: #ebedf2;
        border-color: #dee1e6;
      }
    }
  }
}
</style>
