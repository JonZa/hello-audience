<template>
	<div id="app">
		<h1>Brightness & Contrast Developer Test
			<br>01 Jun, 2018 – 31 Dec, 2019
		</h1>
		<Canvas
			:width="375"
			:height="195"
			:imageFile="this.imageFile"
			:imageData="this.imageData"
			:imageDataFiltered="this.imageDataFiltered"
			v-on:updateImageData="updateImageData"
		/>
		<Avatar :avatarFile="this.avatarFile" :avatarName="this.avatarName"/>
		<div class="slider-block slider-block--green">
			<h2>Contrast</h2>
			<Slider
				:min="-100"
				:max="100"
				:value="this.contrast"
				:step="1"
				name="contrast"
				v-on:updateFilterValue="updateFilterValue"
			/>
			<p>Slide to adjust image brightness! ☀️</p>
		</div>
		<div class="slider-block slider-block--blue">
			<h2>Brightness</h2>
			<Slider
				:min="-100"
				:max="100"
				:value="this.brightness"
				:step="1"
				name="brightness"
				v-on:updateFilterValue="updateFilterValue"
			/>
			<p>Slide to adjust image contrast! 🌓</p>
		</div>
		<div class="upload-block">
			<img src="./assets/cat1.png" alt="Previous image">
			<div class="upload-block__files">
				<Value text="Name" :value="this.imageFileName" class="upload-block__filename"/>
				<File accept=".jpg, .png" v-on:updateFile="updateFile" class="upload-block__fileinput"/>
			</div>
		</div>
	</div>
</template>

<script>
import Canvas from "./components/Canvas.vue";
import Slider from "./components/Slider.vue";
import File from "./components/File.vue";
import Value from "./components/Value.vue";
import Avatar from "./components/Avatar.vue";
export default {
	name: "app",
	components: {
		File,
		Slider,
		Value,
		Avatar,
		Canvas
	},
	data() {
		return {
			brightness: 0,
			contrast: 0,
			avatarName: "Jon",
			avatarFile: require("./assets/jon.jpg"),
			imageFileName: "./assets/cat1.png",
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
		updateFile: function(data) {
			this.imageFile = data.file;
			this.imageFileName = data.filename;
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

<style lang="scss">
	#app {
		background: #fff;
		margin: 50px auto 0 auto;
		padding-bottom: 20px;
		width: 375px;
		font-family: "Graphik-Regular";
		font-weight: normal;
	}
	h1 {
		font-family: "Graphik-Regular";
		font-weight: normal;
		background: #7344c0;
		line-height: 20px;
		text-align: center;
		font-size: 13px;
		padding: 10px;
		color: #fff;
	}
	h2 {
		font-family: "Graphik-Medium";
		font-weight: normal;
		font-size: 18px;
	}
	.upload-block {
		border-radius: 5px;
		box-shadow: inset 1px 0px 0px 0px #dcdee4, inset 0px -1px 0px 0px #dcdee4,
			inset -1px 0px 0px 0px #dcdee4;
		margin: 30px 20px 0 20px;
		overflow: hidden;
		text-transform: uppercase;
		font-size: 11px;
		&__files {
			display: flex;
			justify-content: space-between;
			padding: 10px;
			position: relative;
		}
		&__filename {
			flex: 0 0 calc(75% - 35px);
			max-width: calc(75% - 35px);
			border-radius: 5px;
			background: #fff;
			border: 1px solid #dcdee4;
			color: #25a95b;
			overflow: hidden;
			span {
				padding: 10px;
				color: #8392A6;
				display: inline-block;
				margin-right: 10px;
				position: relative;
					background: #f6f8fa;
					border-right: 1px solid #dcdee4;
			}
		}
		&__fileinput {
			flex: 0 0 25%;
			max-width: 25%;
			overflow: hidden;
		}
		img {
			display: block;
			max-width: 100%;
		}
	}
	.slider-block {
		border-radius: 5px;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
		margin: 0 20px 10px 20px;
		color: #42506b;
		text-align: center;
		font-size: 13px;
		padding: 10px 20px;
		&--green {
			--dark: #25a95b;
			--light: #c7e9d5;
		}
		&--blue {
			--dark: #4a90e2;
			--light: #c7dae9;
		}
		h2 {
			color: var(--dark);
			margin-bottom: 10px;
		}
		[type="range"] {
			margin-bottom: 10px;
		}
	}
</style>
