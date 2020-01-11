<template>
	<div :style="'padding: 0 '+ px(posX())">
		<svg
			width="100%"
			:height="px(height)"
			:class="cssClass"
		>
			<g
				fill="none"
				:stroke="strokeColor"
				:stroke-width="px(fs * 0.60)"
			>
				<line
					v-for="line in lines"
					:x1="0"
					:y1="px(posY(line))"
					x2="100%"
					:y2="px(posY(line))"
				/>
			</g>
		</svg>
	</div>
</template>

<script>
	export default {
		name: 'VcParagraph',

		props: {
			mode: {
				type: String,
				default: 'load'
			},

			lines: {
				type: Number,
				default: 4,
			},

			fontSize: {
				type: Number,
			},

			lineHeight: {
				type: Number,
			},

			strokeColor: {
				type: String,
				default: '#666',
			},
		},

		data: () => ({
			cs: {},
		}),

		computed: {
			fs() {
				return parseFloat(this.fontSize || this.cs.fontSize || 0);
			},

			lh() {
				if (/[a-z]/.test(this.cs.lineHeight))
					this.cs.lineHeight = this.fs * 1.2;

				return parseFloat(this.lineHeight || this.cs.lineHeight || 0);
			},

			height() {
				return this.lh * this.lines;
			},

			cssClass() {
				return 'vc-paragraph '+ this.mode;
			},
		},

		mounted() {
			let { fontSize, lineHeight } = getComputedStyle(this.$el)

			this.cs = {
				fontSize,
				lineHeight,
			};
		},

		methods: {
			px(value) {
				return value +'px';
			},

			posX() {
				return this.fs / 2;
			},

			posY(line) {
				return this.lh * line - this.fs / 2;
			},
		},
	};
</script>

<style lang="scss">
	svg.vc-paragraph {
		overflow: visible;

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
				opacity: 0.3;
			}
		}
	}
</style>
