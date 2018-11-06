<template>
  <div id="app">
    <Transfer :titles="titles">
      <template slot="left-title">
        <span>自定义{{titles[0]}}</span>
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
    return {
      titles: ["左标题", "右标题"]
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
