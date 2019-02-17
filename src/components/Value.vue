<template>
	<div>
		<span class="upload-block__filename-label">{{ text }}</span>
		<span class="upload-block__filename-text">{{ value }}</span>
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

<style>
</style>
