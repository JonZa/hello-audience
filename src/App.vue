<template>
	<div id="app">
		<Canvas
			:width="375"
			:height="195"
			:imageFile="this.imageFile"
			:imageData="this.imageData"
			:imageDataFiltered="this.imageDataFiltered"
			v-on:updateImageData="updateImageData"
		/>
		<hr>
		<Slider
			:min="-100"
			:max="100"
			:value="this.contrast"
			:step="1"
			name="contrast"
			v-on:updateFilterValue="updateFilterValue"
		/>
		<Value text="Contrast" :value="this.contrast"/>
		<hr>
		<Slider
			:min="-100"
			:max="100"
			:value="this.brightness"
			:step="1"
			name="brightness"
			v-on:updateFilterValue="updateFilterValue"
		/>
		<Value text="Brightness" :value="this.brightness"/>
		<hr>
		<File accept=".jpg, .png" v-on:uploadFile="uploadFile"/>
	</div>
</template>

<script>
import Canvas from "./components/Canvas.vue";
import Slider from "./components/Slider.vue";
import File from "./components/File.vue";
import Value from "./components/Value.vue";
export default {
	name: "app",
	components: {
		File,
		Slider,
		Value,
		Canvas
	},
	data() {
		return {
			brightness: 0,
			contrast: 0,
			imageFile: require("./assets/cat1.png"),
			imageData: null,
			imageDataFiltered: null
		};
	},
	watch: {
		brightness: function() {
			this.updateImageDataFiltered(this.imageData.slice(0));
		},
		contrast: function() {
			this.updateImageDataFiltered(this.imageData.slice(0));
		}
	},
	methods: {
		uploadFile: function(file) {
			this.imageFile = file;
		},
		updateImageData: function(imageData) {
			this.imageData = imageData;
			if (this.brightness !== 0 || this.contrast !== 0) {
				this.updateImageDataFiltered(this.imageData.slice(0));
			}
		},
		updateImageDataFiltered: function(imageData) {
			var factor =
				(259.0 * (this.contrast + 255.0)) /
				(255.0 * (259.0 - this.contrast));
			for (var i = 0; i < imageData.length; i += 4) {
				imageData[i] += 255 * (this.brightness / 100);
				imageData[i] = this.truncate(
					factor * (imageData[i] - 128.0) + 128.0
				);
				imageData[i + 1] += 255 * (this.brightness / 100);
				imageData[i + 1] = this.truncate(
					factor * (imageData[i + 1] - 128.0) + 128.0
				);
				imageData[i + 2] += 255 * (this.brightness / 100);
				imageData[i + 2] = this.truncate(
					factor * (imageData[i + 2] - 128.0) + 128.0
				);
			}
			this.imageDataFiltered = imageData.slice(0);
		},
		updateFilterValue: function(key, value) {
			this[key] = value;
		},
		truncate: function(value) {
			if (value < 0) {
				value = 0;
			} else if (value > 255) {
				value = 255;
			}
			return value;
		}
	}
};
</script>

<style>
	#app {
		font-family: "Avenir", Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>
