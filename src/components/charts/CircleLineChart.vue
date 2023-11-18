<script setup>
import { ref, computed } from "vue";

// import MetroCarDensity from '../utilities/MetroCarDensity.vue';
// import { lineInfo } from '../../assets/configs/apexcharts/taipeiMetroLines';

const props = defineProps(["chart_config", "activeChart"]);

const currentIndex = ref(null);

const chartData = ref([
	{ color: "#677693", size: 3, amount: 6000 },
	{ color: "#c2d0bf", size: 4, amount: 4000 },
	{ color: "#71a9ab", size: 6, amount: 6000 },
	{ color: "#95c6f2", size: 4, amount: 6000 },
	{ color: "#507eca", size: 3, amount: 2000 },
	{ color: "#507eca", size: 4, amount: 3000 },
]);

function selectLabel(e, index) {
	currentIndex.value = index;
	mousePosition.value.x = e.pageX;
	mousePosition.value.y = e.pageY;
}

function unSelectLabel() {
	currentIndex.value = null;
}

const mousePosition = ref({ x: null, y: null });

const tooltipPosition = computed(() => {
	return {
		left: `${mousePosition.value.x + 20}px`,
		top: `${mousePosition.value.y - 110}px`,
	};
});
</script>

<template>
	<div v-if="activeChart === 'CircleLineChart'" class="circle-line-chart">
		<div class="time-line"></div>
		<div
			v-for="(data, index) in chartData"
			:key="data.color"
			class="age"
			:style="{
				width: `${20 * data.size}px`,
				height: `${20 * data.size}px`,
				left: `calc(100% / ${chartData.length - 1} * ${index} + 40px)`,
				opacity: currentIndex === index ? '0.9' : '0.6',
				backgroundColor: data.color,
			}"
			@mouseover="(e) => selectLabel(e, index)"
			@mouseleave="unSelectLabel"
		>
			{{ `${index}歲` }}
		</div>
		<div class="label-points">
			<div
				v-for="(data, index) in chartData"
				:key="data.color"
				class="label"
				@mouseover="(e) => selectLabel(e, index)"
				@mouseleave="unSelectLabel"
			>
				<div
					class="label-icon"
					:style="{ backgroundColor: data.color }"
				></div>
				<div class="label-name">{{ data.amount }}</div>
			</div>
		</div>
		<Teleport to="body">
			<!-- The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css -->
			<div
				v-if="currentIndex !== null"
				class="chart-tooltip"
				:style="tooltipPosition"
			>
				<h6>補助細項</h6>
				<span>私立幼兒園：100</span>
				<span>準公共幼兒園：100</span>
				<span>非營利幼兒園：100</span>
				<span>公立幼兒園：100</span>
				<span>總計：1000</span>
			</div>
		</Teleport>
	</div>
</template>

<style scoped lang="scss">
.circle-line-chart {
	position: relative;
	width: 100%;
	height: 100%;
	display: flex;
	align-items: center;

	h6 {
		color: #fff;
		font-size: 20px;
		font-weight: 400;
	}

	.time-line {
		width: 100%;
		height: 20px;
		background-color: #d7e2f8;
	}

	.age {
		position: absolute;
		top: 50%;
		font-size: 12px;
		color: #282a2c;
		border-radius: 50%;
		opacity: 0.9;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: opacity ease-in-out 0.3s;
		transform: translate(-50%, -50%);
		cursor: pointer;
	}

	.label-points {
		position: absolute;
		bottom: 40px;
		left: 30px;
		right: 30px;
		display: flex;
		justify-content: space-between;
		cursor: pointer;
		.label {
			display: flex;
			gap: 4px;
			.label-icon {
				width: 10px;
				height: 10px;
			}
			.label-name {
				font-size: 10px;
				color: #fff;
			}
		}
	}
}

.chart-tooltip {
	position: absolute;
	background-color: white;
	h6 {
		color: white;
		font-size: 0.75rem;
	}
	span {
		display: block;
		font-size: 1rem;
	}
}
</style>
