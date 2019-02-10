<template>
	<input
		type="range"
		:min="min"
		:max="max"
		:step="step"
		v-model.number="sliderValue"
		v-on:input="$emit('updateFilterValue', name, sliderValue)"
	>
</template>
 
<script>
export default {
	name: "Slider",
	data() {
		return {
			sliderValue: this.value
		};
	},
	props: {
		name: String,
		min: Number,
		value: Number,
		step: Number,
		max: Number
	}
};
</script> 
 
<style lang="scss">
	// Styling Cross-Browser Compatible Range Inputs with Sass
	// Github: https://github.com/darlanrod/input-range-sass
	// Author: Darlan Rod https://github.com/darlanrod
	// Version 1.5.1
	// MIT License

	$track-color: var(--light) !default;
	$thumb-color: var(--dark) !default;

	$thumb-radius: 50% !default;
	$thumb-height: 22px !default;
	$thumb-width: 22px !default;
	$thumb-shadow-size: 0 !default;
	$thumb-shadow-blur: 0 !default;
	$thumb-shadow-color: rgba(0, 0, 0, 0.2) !default;
	$thumb-border-width: 3px !default;
	$thumb-border-color: #fff !default;

	$track-width: 100% !default;
	$track-height: 8px !default;
	$track-shadow-size: 0 !default;
	$track-shadow-blur: 0 !default;
	$track-shadow-color: rgba(0, 0, 0, 0.2) !default;
	$track-border-width: 0 !default;
	$track-border-color: #f00 !default;

	$track-radius: 5px !default;
	$contrast: 5% !default;

	$ie-bottom-track-color: $track-color !default;

	@mixin shadow($shadow-size, $shadow-blur, $shadow-color) {
		// box-shadow: $shadow-size $shadow-size $shadow-blur $shadow-color, 0 0 $shadow-size lighten($shadow-color, 5%);
	}

	@mixin track {
		cursor: default;
		height: $track-height;
		transition: all 0.2s ease;
		width: $track-width;
	}

	@mixin thumb($adjustment: 0) {
		@include shadow(
			$thumb-shadow-size,
			$thumb-shadow-blur,
			$thumb-shadow-color
		);
		background: $thumb-color;
		border: $thumb-border-width solid $thumb-border-color;
		border-radius: $thumb-radius;
		cursor: default;
		height: $thumb-height + $adjustment;
		width: $thumb-width + $adjustment;
	}

	@mixin disabled {
		cursor: not-allowed;
	}

	[type="range"] {
		-webkit-appearance: none;
		background: transparent;
		// margin: $thumb-height / 2 0;
		width: $track-width;

		&::-moz-focus-outer {
			border: 0;
		}

		&:focus {
			outline: 0;

			&::-webkit-slider-runnable-track {
				background: $track-color;
			}

			&::-ms-fill-lower {
				background: $track-color;
			}

			&::-ms-fill-upper {
				background: $track-color;
			}
		}

		&::-webkit-slider-runnable-track {
			@include track;
			@include shadow(
				$track-shadow-size,
				$track-shadow-blur,
				$track-shadow-color
			);
			background: $track-color;
			border: $track-border-width solid $track-border-color;
			border-radius: $track-radius;
		}

		&::-webkit-slider-thumb {
			@include thumb;
			-webkit-appearance: none;
			margin-top: (
				(-$track-border-width * 2 + $track-height) / 2 - $thumb-height / 2
			);
		}

		&::-moz-range-track {
			@include shadow(
				$track-shadow-size,
				$track-shadow-blur,
				$track-shadow-color
			);
			@include track;
			background: $track-color;
			border: $track-border-width solid $track-border-color;
			border-radius: $track-radius;
			height: $track-height / 2;
		}

		&::-moz-range-thumb {
			@include thumb(-4);
		}

		&::-ms-track {
			@include track;
			background: transparent;
			border-color: transparent;
			border-width: ($thumb-height / 2) 0;
			color: transparent;
		}

		&::-ms-fill-lower {
			@include shadow(
				$track-shadow-size,
				$track-shadow-blur,
				$track-shadow-color
			);
			background: $ie-bottom-track-color;
			border: $track-border-width solid $track-border-color;
			border-radius: ($track-radius * 2);
		}

		&::-ms-fill-upper {
			@include shadow(
				$track-shadow-size,
				$track-shadow-blur,
				$track-shadow-color
			);
			background: $track-color;
			border: $track-border-width solid $track-border-color;
			border-radius: ($track-radius * 2);
		}

		&::-ms-thumb {
			@include thumb(-4);
			margin-top: $track-height / 4;
		}

		&:disabled {
			&::-webkit-slider-thumb {
				@include disabled;
			}

			&::-moz-range-thumb {
				@include disabled;
			}

			&::-ms-thumb {
				@include disabled;
			}

			&::-webkit-slider-runnable-track {
				@include disabled;
			}

			&::-ms-fill-lower {
				@include disabled;
			}

			&::-ms-fill-upper {
				@include disabled;
			}
		}
	}
</style> 