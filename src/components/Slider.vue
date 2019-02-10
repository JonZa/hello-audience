<template>
	<input
		type="range"
		:min="min"
		:max="max"
		:step="step"
		v-model.number="sliderValue"
		v-on:input="$emit('updateFilterValue', name, sliderValue)"
	>
</template>
 
<script>
export default {
	name: "Slider",
	data() {
		return {
			sliderValue: this.value
		};
	},
	props: {
		name: String,
		min: Number,
		value: Number,
		step: Number,
		max: Number
	}
};
</script> 
 
<style>
</style> 