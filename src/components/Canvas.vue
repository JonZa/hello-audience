<template>
	<canvas ref="canvas" :width="width" :height="height"/>
</template>
 
<script>
export default {
	name: "Canvas",
	props: {
		width: Number,
		height: Number,
		imageFile: String,
		imageData: Uint8ClampedArray,
		imageDataFiltered: Uint8ClampedArray
	},
	methods: {
		drawImage: function(imageFile) {
			var $this = this;
			var ctx = this.$refs["canvas"].getContext("2d");
			var image = new Image();
			image.onload = function() {
				var imageHeight = image.height * ($this.width / image.width);
				var imageY = ($this.height - imageHeight) / 2;
				ctx.clearRect(0, 0, $this.width, $this.height);
				ctx.drawImage(image, 0, imageY, $this.width, imageHeight);
				$this.$emit(
					"updateImageData",
					ctx.getImageData(0, 0, $this.width, $this.height).data
				);
			};
			image.src = imageFile;
		},
		drawData: function(imageData) {
			var ctx = this.$refs["canvas"].getContext("2d");
			var createdImageData = ctx.createImageData(this.width, this.height);
			createdImageData.data.set(imageData);
			ctx.putImageData(createdImageData, 0, 0);
		}
	},
	watch: {
		imageFile: function(val) {
			this.drawImage(val);
		},
		imageData: function(val) {
			this.drawData(val);
		},
		imageDataFiltered: function(val) {
			this.drawData(val);
		}
	},
	mounted: function() {
		this.drawImage(this.imageFile);
	}
};
</script> 
 
<style scoped>
	canvas {
		border: 1px solid #000;
	}
</style> 