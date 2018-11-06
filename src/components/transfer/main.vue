<template>
  <div class="pvui-transfer">
    <transfer-panel :title="titles[0]">
      <template slot="title">
        <slot name="left-title"></slot>
      </template>
    </transfer-panel>
  </div>
</template>
<script>
import TransferPanel from "./transfer-panel.vue";
export default {
	name: "Transfer",
	props: {
		titles: {
			type: Array,
			default() {
				return [];
			}
		}
	},
	components: {
		TransferPanel
	}
};
</script>
<style lang="less" scoped>
.pvui-transfer {
	font-size: 40px;
}
</style>
