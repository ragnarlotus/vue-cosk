<template>
	<svg
		v-if="mounted"
		:width="px(size.width)"
		:height="px(size.height)"
		class="vue-cosk"
		:class="cssClass">

		<line
			v-if="type === 'paragraph'"
			v-for="line in lines"
			:x1="px(posX())"
			:y1="px(posY(line))"
			:x2="px(size.width - posX())"
			:y2="px(posY(line))"
			:stroke="strokeColor"
			:stroke-width="px(fontSize)"
		/>
	</svg>
</template>

<script>
	export default {
		name: 'VueCosk',

		data: () => ({
			mounted: false,
		}),

		props: {
			mode: {
				type: String,
				default: 'load'
			},

			type: {
				type: String,
				default: 'paragraph',
			},

			lines: {
				type: Number,
				default: 4,
			},

			fontSize: {
				type: Number,
				default: 12,
			},

			lineHeight: {
				type: Number,
				default: 1.5,
			},

			strokeColor: {
				type: String,
				default: '#666',
			},
		},

		computed: {
			parent() {
				return this.$el.parentNode;
			},

			size() {
				return {
					width: this.parent.clientWidth,
					height: this.lineHeight * this.fontSize * this.lines,
				};
			},

			cssClass() {
				return this.mode;
			},

			strokeWidth() {
				return this.px(this.fontSize);
			},
		},

		mounted() {
			this.mounted = true;
		},

		methods: {
			px(value) {
				return value +'px';
			},

			posX() {
				return this.fontSize / 2;
			},

			posY(line) {
				let height = this.fontSize * this.lineHeight;
				return height * line - height / 2;
			},
		},
	};
</script>

<style lang="scss">
	svg.vue-cosk {
		line {
			stroke-linecap: round;
		}

		&.load {
			line {
				animation: fadeInOut 1s linear infinite alternate;
			}
		}

		@keyframes fadeInOut {
			from {
				opacity: 1;
			}

			to {
				opacity: 0.2;
			}
		}
	}
</style>
