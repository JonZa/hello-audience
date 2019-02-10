<template>
	<input type="file" :accept="accept" v-on:change="uploadFile">
</template>
 
<script>
export default {
	data() {
		return {
			file: null
		};
	},
	methods: {
		uploadFile(event) {
			var file = event.target.files[0];
			var fileReader = new FileReader();
			fileReader.onload = (event) => {
				this.$emit('uploadFile',event.target.result);
			};
			fileReader.readAsDataURL(file);
		}
	},
	props: {
		accept: String
	}
};
</script> 
 
<style>
</style> 