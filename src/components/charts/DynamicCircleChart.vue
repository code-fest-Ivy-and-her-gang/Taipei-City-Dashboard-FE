<script setup>
import { ref } from "vue";

// import MetroCarDensity from '../utilities/MetroCarDensity.vue';
// import { lineInfo } from '../../assets/configs/apexcharts/taipeiMetroLines';

const props = defineProps(["chart_config", "activeChart"]);

const color = ref(props.chart_config.color[0]);
</script>

<template>
	<div v-if="activeChart === 'DynamicCircleChart'">
		這是動態圓餅圖 {{ color }}
	</div>
</template>

<style scoped lang="scss">
.circle-line-chart {
	width: 100%;
}
</style>
