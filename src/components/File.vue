<template>
	<div>
		<button>Upload</button>
		<input type="file" :accept="accept" v-on:change="updateFile">
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
 
<style lang="scss" scoped>
	div {
		button {
			border-radius: 5px;
			background: #fff;
			border: 1px solid #dcdee4;
			color: #4A90E2;
			padding: 10px;
			width: 100%;
			text-transform: inherit;
			font-size: inherit;
		}
		[type="file"] {
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			opacity: 0;
			width: 100%;
		}
	}
</style> 