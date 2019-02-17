<template>
	<div>
		<label for="upload-block__file-input" class="upload-block__label">Upload</label>
		<input id="upload-block__file-input" type="file" :accept="accept" v-on:change="updateFile">
	</div>
</template>
 
<script>
export default {
	name: "File",
	methods: {
		updateFile(event) {
			var file = event.target.files[0];
			var fileReader = new FileReader();
			fileReader.onload = event => {
				this.$emit("updateFile", {
					file: event.target.result,
					filename: file.name
				});
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