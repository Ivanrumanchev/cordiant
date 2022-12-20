<template lang="pug">
.page-game
	Header.page-game--header

	main.main
		HalfLayout
			template(v-slot:left)
				picture
					source(type="image/webp" :srcset="`${getMirrorWEBP} 1x, ${getMirrorWEBP} 2x`")
					img.main__mirror(:src="getMirrorPNG" :srcset="`${getMirrorPNG} 2x`" alt="mirror" width="128" height="59")

				.main__cabin

				picture
					source(type="image/webp" :srcset="`${getPictureWEBP} 1x, ${getPictureWEBP} 2x`")
					img.main__picture(:src="getPictureJPG" :srcset="`${getPictureJPG} 2x`" :alt="`question-${step}`" width="320" height="194")

			template(v-slot:right)
				.container
					.main__info-card.info-card
						.info-card__step {{`${step}/${Object.keys(info.main.info).length}`}}

						template(v-if="isQuestion")
							h1.info-card__title
								SvgEl(:name="info.main.title.svg")

							ul.info-card__answer-list
								li.info-card__answer-item(v-for="(answer, idx) of info.main.info['step-' + step]" :key="idx" @click="onAnswerClick(answer)" v-html="answer.answer")

						template(v-else)
							p.info-card__result {{currentAnswer.result}}

							p.info-card__extra(v-if="currentAnswer.extra" v-html="currentAnswer.extra")

							Button.info-card__next(:info="info.main.link" @click="onNextClick")

	Footer(withoutShare).page-game--footer
</template>

	<script>
	import Default from '@/layouts/Default.vue';
	import HalfLayout from '@/layouts/HalfLayout.vue';
	import SvgEl from '@/core/SvgEl.vue';
	import Button from '@/core/Button.vue';
	import Header from '@/components/Header/Header.vue';
	import Footer from '@/components/Footer.vue';

	export default {
		name: 'game',

		components: {
			Default,
			HalfLayout,
			SvgEl,
			Button,
			Header,
			Footer,
		},

		data: (context) => ({
			info: context.$utils.loadJSON('gamePage.json'),
			step: 1,
			countCorrect: 0,
			isQuestion: true,
			currentAnswer: null,
		}),

		computed: {
			getPictureJPG() {
				return this.$utils.loadAsset(`question-${this.step}.jpg`);
			},
			getPictureWEBP() {
				return this.$utils.loadAsset(`question-${this.step}.webp`);
			},
			getMirrorPNG() {
				const emoji = this.currentAnswer ? this.currentAnswer.isCorrect : 'question';

				return this.$utils.loadAsset(`mirror-${emoji.toString()}.png`);
			},
			getMirrorWEBP() {
				const emoji = this.currentAnswer ? this.currentAnswer.isCorrect : 'question';

				return this.$utils.loadAsset(`mirror-${emoji.toString()}.webp`);
			},
		},

		methods: {
			onAnswerClick(answer) {
				this.currentAnswer = answer;
				this.isQuestion = false;

				if (answer.isCorrect) {
					this.countCorrect++;
				}
			},
			onNextClick() {
				this.isQuestion = true;
				this.currentAnswer = null;
				this.step++;
			},
		}
	};
	</script>

	<style lang="scss" scoped>
		.page-game{
			flex-grow: 1;
			@include flex(flex, $dir: column);

			&--header {
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				z-index: 100;
			}
		}

		.main {
			flex-grow: 1;
			@include flex(flex, center, flex-start, column);
			background-color: $blue;
			position: relative;

			&__mirror {
				position: absolute;
				top: 0;
				left: 50%;
				transform: translateX(-50%);
				width: rem(128);
				height: rem(51);

				@include breakpoint('md') {
					transform: translateX(-25%);
					width: rem(320);
					height: rem(148);
				}
			}

			&__cabin {
				display: none;

				@include breakpoint('md') {
					display: block;
					position: absolute;
					bottom: 0;
					left: 0;
					width: 100%;
					height: rem(426);
					background: url("../assets/images/cabin-question.png") no-repeat;
					background-size: cover;

					@media (min-resolution: 144dpi),
						(min-resolution: 1.5dppx) {
						background: url("../assets/images/cabin-question@2x.png") no-repeat;
						background-size: cover;
					}
				}
			}

			&__picture {
				width: 100%;
				max-width: 100%;
				object-fit: cover;

				@include breakpoint('md') {
						height: 100vh;
					}
			}

			.container {
        @include breakpoint('md') {
          width: auto;
        }
      }
		}

		.info-card {
			position: relative;

			&__step {
				margin-top: rem(23);
				color: $white;

				@include breakpoint('md') {
          margin-top: rem(78);
        }
			}

			&__title {
				margin-top: rem(16);

				@include breakpoint('md') {
          margin-top: rem(27);
        }

				svg {
					margin: 0 auto;
					width: rem(284);
					height: rem(43);

					@include breakpoint('md') {
						width: rem(345);
						height: rem(52);
					}
				}
			}

			&__answer-list {
				width: 100%;
				margin-top: rem(6);
				padding: rem(29) 0 rem(31) 0;
				color: $white;

				@include fs(14, 24);

				@include breakpoint('md') {
          padding-top: rem(20);
					width: rem(456);
					@include fs(16, 26);
        }
			}

			&__answer-item {
				min-height: rem(82);
				background-color: $answer-color;
				padding: rem(16) rem(20);
				border-radius: 3px;

				&:not(:last-child) {
					margin-bottom: rem(16);
				}

				@include hover {
					background-color: $light-blue;
					color: $dark-blue-text;
				}

				&:active {
					background-color: $white;
					color: $dark-blue-text;
				}
			}

			&__result {
				margin: rem(16) rem(16) rem(24) rem(16);

				@include fs(18, 26, 700);

				color: $white;

				@include breakpoint('md') {
          margin-top: rem(85);
        }
			}

			&__extra {
				margin: 0 rem(16) 0 rem(16);
				font-weight: 400;
				color: $white;

				@include breakpoint('md') {
          width: rem(370);
        }

				::v-deep .link {
					color: $light-blue;
					text-decoration: 1px solid $light-blue;
					text-decoration-line: underline;
					font-weight: 700;
				}
			}

			&__next {
				margin: rem(40) auto 0 auto;
				display: block;
				width: rem(250);
				font-weight: 700;
			}
		}
	</style>
