<template>
	<img :src="avatarFile" :alt="avatarName">
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

<style scoped>
    img {
        width: 60px;
        height: 60px;
        border-radius: 50%;
        border: 2px solid #fff;
        display: block;
        margin: -50px auto 10px auto;
        transform: translatez(0);
    }
</style>
