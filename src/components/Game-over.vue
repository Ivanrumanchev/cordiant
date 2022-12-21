<template lang="pug">
.game-over
	.game-over__top-section
		.container
			h1.game-over__title {{getTitle}}

			p.game-over__description {{getDescription}}

			.game-over__action.action
				Share.action__share(:theme="'blue'", rounded)
				button.action__repeat(@click="onRepeatClick")
					SvgEl(:name="info.main.button.repeat.svg")
					span {{info.main.button.repeat.text}}

	.game-over__bottom-section
		.container
			p.game-over__extra(v-html="getExtra")

			Button.game-over__pay(:info="info.main.linkPay")
</template>

<script>
import SvgEl from '@/core/SvgEl.vue';
import Share from '@/core/Share.vue';
import Button from '@/core/Button.vue';

export default {
	name: 'game-over',

	props: {
		result: {
			type: String,
			required: true,
		},
	},

	components: {
		SvgEl,
		Share,
		Button,
	},

	data: (context) => ({
		info: context.$utils.loadJSON('gamePage.json'),
		title: '',
		description: '',
	}),

	computed: {
		getTitle() {
			return this.info.main.results[this.result].title;
		},

		getDescription() {
			return this.info.main.results[this.result].description;
		},

		getExtra() {
			return this.info.main.results[this.result].extraInfo;
		},
	},

	methods: {
		onRepeatClick() {
			this.$emit('repeat');
		},
	},
};
</script>

<style lang="scss" scoped>
	.game-over {
		&__top-section {
			border-bottom: 2px solid $white;
		}

		.container {
			@include breakpoint('md') {
				width: rem(428);
			}
		}

		&__title {
			margin-top: rem(40);
			color: $white;
			font-family: 'play', $Arial;

			@include fs(35, 40, 700);

			@include breakpoint('md') {
				margin-top: rem(118);
				width: rem(428);
			}
		}

		&__description {
			margin: rem(16) 0 rem(40) 0;

			@include fs(18, 26, 700);

			color: $white;

			@include breakpoint('md') {
				margin-bottom: rem(16);
				width: rem(428);
			}
		}

		&__extra {
			margin: rem(24) 0 rem(40) 0;
			font-weight: 400;
			color: $white;

			@include breakpoint('md') {
				width: rem(428);
			}

			::v-deep .link {
				color: $light-blue;
				text-decoration: 1px solid $light-blue;
				text-decoration-line: underline;
				font-weight: 700;
			}

			::v-deep .promo {
				padding: 2px 4px;
				background-color: $promo-bg-color;
				border-radius: 3px;
				color: $promo-text-color;
				font-weight: 700;
			}

			::v-deep .due-date {
				color: $promo-text-color;
				font-weight: 700;
			}
		}

		&__pay {
			display: block;
			font-weight: 700;

			display: block;
      margin: 0 auto;

      @include breakpoint('md') {
        margin-left: 0;
        margin-right: auto;
				margin-bottom: rem(92);
        width: rem(282);
      }
		}
	}
	.action {
		@include flex(flex, center, space-between, $dir: column);

		@include breakpoint('sm') {
			flex-direction: row;
		}

		@include breakpoint('md') {
			width: rem(360);
		}

		&__share {
			margin-bottom: rem(27);

			@include breakpoint('md') {
        order: 2;
      }
		}

		&__repeat {
			@include flex(flex, center, center);
			margin-bottom: rem(24);
			gap: rem(16);

			@include hover() {
				span {
					color: $white;
				}

				svg {
					fill: $white;
				}
			}

			svg {
				width: rem(40);
				height: rem(35);
				fill: $light-blue;
			}

			span {
				font-family: 'roboto', $Arial;
				color: $light-blue;
				@include fs(16, 24, 700);
			}
		}
	}
</style>
