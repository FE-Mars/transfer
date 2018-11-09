<template>
  <div id="app">
    <Transfer 
      v-model="value"
      :titles="titles" 
      :data="data"
      :before-right-change="beforeRightChange"
      :groupable="true">
      <template slot="right-title">
        <span>自定义{{titles[1]}}-{{value.length}}/5</span>
      </template>
    </Transfer>
  </div>
</template>

<script>
import Transfer from "./components/transfer";

export default {
  name: "App",
  components: {
    Transfer
  },
  data() {
    const data = [];
    for (let i = 1; i <= 15; i++) {
      data.push({
        key: i,
        label: `第一个分组 ${i}`
      });
    }
    const data2= [];
    for (let i = 1; i <= 15; i++) {
      data2.push({
        key: 'num' + i,
        label: `第二个分组 ${i}`
      });
    }
    return {
      titles: ["左标题", "右标题"],
      data: [{
          title: '测试分组标题',
          items: data
        },
        {
          title: '测试分组标题2',
          items: data2
        }
      ],
      value: [1, 'num2']
    };
  },
  methods: {
    beforeRightChange(currentValue, addValues) {
      if(currentValue.length + addValues.length > 5){
        this.$alert('❌')
        return false;
      }
        
      return true;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
