<template>
	<img :src="avatarFile" :alt="avatarName" class="avatar">
</template>

<script>
export default {
	name: "Avatar",
	props: {
		avatarFile: String,
		avatarName: String
	}
};
</script>

<style>
</style>
