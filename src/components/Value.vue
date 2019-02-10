<template>
	<div>
		<span>{{ text }}</span>
		{{ value }}
	</div>
</template>

<script>
export default {
	name: "Value",
	props: {
		text: String,
		value: String
	}
};
</script>

<style scoped>
</style>
