<template>
	<h1>{{ text }}: {{ value }}</h1>
</template>

<script>
export default {
	name: "Value",
	props: {
		text: String,
		value: Number
	}
};
</script>

<style scoped>
</style>
