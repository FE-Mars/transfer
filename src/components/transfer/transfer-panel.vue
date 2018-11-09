<template>
  <div class="pvui-transfer-panel">
    <div class="pvui-transfer-panel-title">
        <slot name="title">{{title}}</slot>
    </div>
    <div class="pvui-transfer-panel-main">
      <div class="pvui-transfer-panel-header">
        <el-checkbox
          class="pvui-transfer-selectall"
          :indeterminate="isIndeterminate" 
          v-model="allChecked" 
          @change="handleAllCheckedChange">
          {{selectAllText}} 
          <span class="pvui-transfer-selectall-summary">{{ checkedSummary }}</span>
        </el-checkbox>
        <el-input
          class="pvui-transfer-panel-search"
          v-model="query"
          :placeholder="placeholder"
          size="small"
          clearable>
        </el-input>
      </div>
      <div class="pvui-transfer-panel-body">
        <el-collapse v-if="groupable" v-model="activeNames">
          <el-collapse-item v-for="(group,index) in parsedData" :key="group.title" :title="group.title" :name="index">
            <transfer-list :data="group.items" :default-checked="checked" @change="handleOptionChange">
              <template slot="content" slot-scope="{data}">
                <slot name="content" :data="data"></slot>
              </template>
            </transfer-list>
          </el-collapse-item>
        </el-collapse>
        <template v-else>
            <transfer-list :data="parsedData" :default-checked="checked" @change="handleOptionChange">
            </transfer-list>
        </template>
      </div>
    </div>
  </div>
</template>
<script>
import TransferList from "./transfer-list.vue";
export default {
  name: "TransferPanel",
  components: {
    TransferList
  },
  props: {
    title: String,
    placeholder: {
      type: String,
      default: '请输入搜索内容'
    },
    groupable: Boolean,
    data: Array,
    defaultChecked: Array,
    filterMethod: Function,
    format: Object,
    props: Object,
    selectAllText: {
      type: String,
      default: '全选'
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
      query: '',
      activeNames: [0],
      allChecked: false,
      checked: [],
      ...getCustomProp(this.props)
    };
  },
  computed: {
    dataLength() {
      let len = 0;
      this.data.forEach(group => {
        len += group[this.itemsProp].length;
      })
      return len;
    },
    parsedData() {
      let _data = []; 
      this.data.forEach(group => {
        let title = group[this.titleProp],
          items = group[this.itemsProp];
        let _items = [];
        items.forEach(item => {
          if((typeof this.filterMethod === 'function' && this.filterMethod(this.query, item, title)) || ~item[this.labelProp].toLowerCase().indexOf(this.query.toLowerCase())) {
            _items.push({
              [this.keyProp]: item[this.keyProp],
              [this.labelProp]: item[this.labelProp]
            }); 
          }
        });
        if(this.groupable){
          _data.push({ title, items: _items});
        }else{
          _data = _data.concat(_items);
        }
      })
      return _data;
    },
    checkedSummary() {
      const checkedLength = this.checked.length;
      const dataLength = this.dataLength;
      const { noChecked, hasChecked } = this.format;
      if (noChecked && hasChecked) {
        return checkedLength > 0
          ? hasChecked.replace(/\${checked}/g, checkedLength).replace(/\${total}/g, dataLength)
          : noChecked.replace(/\${total}/g, dataLength);
      } else {
        return `${ checkedLength }/${ dataLength }`;
      }
    },
    isIndeterminate() {
      const checkedLength = this.checked.length;
      return checkedLength > 0 && checkedLength < this.dataLength;
    },
  },
  watch: {
    defaultChecked: {
      immediate: true,
      handler(val, oldVal) {
        this.checked = [...val];
      }
    },
    checked() {
      const checkedLength = this.checked.length;
      this.allChecked = checkedLength > 0 && checkedLength === this.dataLength;
    }
  },
  methods: {
    handleOptionChange(value) {
      this.checked = [...value];
      this.$emit("change", this.checked);
    },
    handleAllCheckedChange(value) {
      this.checked = value
          ? this.parsedData.map(item => item[this.keyProp])
          : [];
      let checked = []
      if(value){
        if(this.groupable){
          this.parsedData.forEach(({items}) => checked = checked.concat(items.map(item => item[this.keyProp])));
        }else{
          checked = this.parsedData.map(item => item[this.keyProp]);
        }
      }
      this.checked = checked;
      this.$emit("change", this.checked);
    }
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
    flex-direction: column;
    height: 260px;
    border-radius: 3px;
    border: solid 1px #dddddd;
    background-color: #ffffff;
    box-sizing: border-box;
    .pvui-transfer-panel-header {
      display: flex;
      flex-direction: column;
      background-color: #ffffff;

      .pvui-transfer-selectall{
        height: 36px;
        line-height: 36px;
        padding: 0 8px;
        border-bottom: 1px solid #dddddd;
        .pvui-transfer-selectall-summary{
          position: absolute;
          right: 8px;
          color: #191c21;
          font-size: 12px;
          font-weight: 400;
        }
      }
      .pvui-transfer-panel-search{
        flex: 1;
        width: auto;
        margin: 10px 8px;
      }
    }
    .pvui-transfer-panel-body {
      flex: 1;
      overflow: auto;
      padding: 0 8px;
      .el-collapse {
        text-align: left;
        // border: none;
        .el-collapse-item__header {
          height: 32px;
          line-height: 32px;
          //   border: none;
          .el-collapse-item__arrow {
            line-height: 32px;
          }
        }
        .el-collapse-item__content {
          padding-bottom: 0;
        }
        // .el-collapse-item__wrap {
        //   border: none;
        // }
      }
    }
  }
}
</style>
