<template>
	<div id="app">
		<h1>
			Brightness & Contrast Developer Test
			<small>01 Jun, 2018 – 31 Dec, 2019</small>
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
			<h2>Brightness</h2>
			<Slider
				:min="-100"
				:max="100"
				:value="this.brightness"
				:step="1"
				name="brightness"
				v-on:updateFilterValue="updateFilterValue"
			/>
			<p>Slide to adjust image brightness! ☀️</p>
		</div>
		<div class="slider-block slider-block--blue">
			<h2>Contrast</h2>
			<Slider
				:min="-100"
				:max="100"
				:value="this.contrast"
				:step="1"
				name="contrast"
				v-on:updateFilterValue="updateFilterValue"
			/>
			<p>Slide to adjust image contrast! 🌓</p>
		</div>
		<div class="upload-block">
			<img src="./assets/images/cat1.png" alt="Previous image" class="upload-block__image">
			<div class="upload-block__files">
				<Value text="Name" :value="this.imageFileName" class="upload-block__filename"/>
				<File accept=".jpg, .png" v-on:updateFile="updateFile" class="upload-block__fileinput"/>
			</div>
		</div>
		<div class="pixel-perfect">
			<input type="checkbox" id="pixel-perfect" class="pixel-perfect__checkbox">
			<label for="pixel-perfect">Pixel test</label>
			<div class="pixel-perfect__image"/>
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
			avatarFile: require("./assets/images/jon.jpg"),
			imageFileName: "A LONG FILE NAME THAT IS LONG.JPG",
			imageFile: require("./assets/images/cat1.png"),
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
	* {
		margin: 0;
		border: 0;
		padding: 0;
	}
	*,
	*:before,
	*:after {
		box-sizing: border-box;
	}
	body {
		background: #fafafa;
	}
	@font-face {
		font-family: "Graphik";
		font-weight: 400;
		font-display: swap;
		src: url("./assets/fonts/Graphik-Regular.woff2") format("woff2");
	}
	@font-face {
		font-family: "Graphik";
		font-weight: 700;
		font-display: swap;
		src: url("./assets/fonts/Graphik-Medium.woff2") format("woff2");
	}
	#app {
		background: #fff;
		margin: 50px auto 0 auto;
		width: 375px;
		font-family: "Graphik";
		font-weight: normal;
		position: relative;
	}
	h1 {
		font-weight: 700;
		background: #7344c0;
		line-height: 20px;
		text-align: center;
		font-size: 15px;
		padding: 16px 10px 14px 10px;
		color: #fff;
		small {
			display: block;
			font-weight: 400;
			font-size: 13px;
		}
	}
	h2 {
		font-size: 18px;
	}
	.avatar {
		width: 64px;
		height: 64px;
		border-radius: 50%;
		border: 2px solid #fff;
		display: block;
		margin: -56px auto 5px auto;
		transform: translatez(0);
	}
	.slider-block {
		border-radius: 5px;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.125);
		margin: 0 20px 7px 20px;
		color: #42506b;
		text-align: center;
		font-size: 13px;
		padding: 10px 20px 3px 20px;
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
			font-weight: 700;
			margin-bottom: 10px;
		}
		&__pseudo-slider {
			position: relative;
			z-index: 0;
			margin-bottom: 10px;
			&::before,
			&::after {
				border-radius: 10px;
				height: inherit;
				position: absolute;
				height: 5px;
				top: 6px;
				left: 0;
				display: block;
				content: "";
				z-index: -1;
			}
			&::before {
				background: var(--light);
				width: 100%;
			}
			&::after {
				width: calc((50% + var(--width)) + var(--offset));
				background: var(--dark);
			}
		}
	}
	.upload-block {
		border-radius: 5px;
		box-shadow: inset 1px 0px 0px 0px #dcdee4, inset 0px -1px 0px 0px #dcdee4,
			inset -1px 0px 0px 0px #dcdee4;
		margin: 29px 20px 0 20px;
		overflow: hidden;
		text-transform: uppercase;
		font-size: 13px;
		font-weight: 700;
		&__image {
			display: block;
			object-fit: cover;
			object-position: center;
			width: 100%;
			height: 212px;
		}
		&__files {
			display: flex;
			justify-content: space-between;
			padding: 18px 9px 7px 8px;
			position: relative;
			span,
			button {
				padding-top: 9px;
				padding-bottom: 8px;
				font-size: 11px;
			}
		}
		&__filename {
			flex: 0 0 calc(75% - 32px);
			display: flex;
			max-width: calc(75% - 32px);
			border-radius: 5px;
			background: #fff;
			border: 1px solid #dcdee4;
			color: #25a95b;
			overflow: hidden;
			letter-spacing: 1px;
			&-label {
				width: 60px;
				color: #8392a6;
				display: inline-block;
				position: relative;
				background: #f6f8fa;
				border-right: 1px solid #dcdee4;
				padding-left: 10px;
			}
			&-text {
				width: calc(100% - 65px);
				padding-left: 5px;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
			}
		}
		&__fileinput {
			flex: 0 0 87px;
			max-width: 87px;
			overflow: hidden;
		}
		&__label {
			border-radius: 5px;
			background: #fff;
			border: 1px solid #dcdee4;
			color: #4a90e2;
			width: 100%;
			text-transform: inherit;
			font-size: 11px;
			padding-bottom: 7px;
			font-weight: 700;
			text-align: right;
			padding-right: 9px;
			padding-top: 9px;
			letter-spacing: 1px;
			display: block;
			&::before {
				display: inline-block;
				content: "";
				width: 0;
				height: 0;
				border-style: solid;
				border-radius: 2px;
				border-width: 0 6px 9px 6px;
				margin-right: 5px;
				border-color: transparent transparent #007bff transparent;
				transform: rotate(360deg);
			}
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
	.pixel-perfect {
		background: #fafafa;
		color: #666;
		margin-top: 20px;
		padding: 10px 20px;
		font-size: 14px;
		text-transform: uppercase;
		&__checkbox {
			position: relative;
			top: 2px;
			margin-right: 10px;
			opacity: 0.5;
		}
		&__image {
			background: url(./assets/images/pixel-perfect.png);
			filter: invert(100%);
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			height: 782px;
			opacity: 0.5;
			display: none;
			@at-root input:checked ~ & {
				display: block;
			}
		}
	}
</style>
