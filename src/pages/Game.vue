<template lang="pug">
.page-game
	Header.page-game--header

	main.main(v-animate.fadeIn="")
		HalfLayout
			template(v-slot:left)
				template(v-if="!isGameOver")
					picture
						source(type="image/webp" :srcset="`${getMirrorWEBP} 1x, ${getMirrorWEBP} 2x`")
						img.main__mirror(:src="getMirrorPNG" :srcset="`${getMirrorPNG} 2x`" alt="mirror" width="128" height="59")

					.main__cabin

				picture(v-animate.fadeIn="" :key="getPictureJPG")
					source(type="image/webp" :srcset="`${getPictureWEBP} 1x, ${getPictureWEBP} 2x`")
					img.main__picture(:src="getPictureJPG" :srcset="`${getPictureJPG} 2x`" :alt="`question-${step}`" width="320" height="194")

			template(v-slot:right)
				Transition(name="fade" mode="out-in")
					.container(v-if="!isGameOver")
						.main__info-card.info-card
							.info-card__step {{`${step} / ${Object.keys(info.main.info).length}`}}

							AnswerVariant(v-if="isQuestion" v-animate.fadeIn="" :info="info" :step="step" @answer-click="onAnswerClick")

							Result(v-else v-animate.fadeIn="" :info="info" :current-answer="currentAnswer" @next-click="onNextClick")

					GameOver(v-else :result="getResult" @repeat="onRepeatClick")

	p.page-game__team(v-if="isGameOver") {{info.main.team}}
	img.page-game__popup(src="../assets/images/popup.png")

	Footer(withoutShare :class="{'page-game--footer': isGameOver}")
</template>

	<script>
	import HalfLayout from '@/layouts/HalfLayout.vue';
	import Header from '@/components/Header/Header.vue';
	import Footer from '@/components/Footer.vue';
	import GameOver from '@/components/Game-over.vue';
	import AnswerVariant from '@/components/Answer-variant.vue';
	import Result from '@/components/Result.vue';

	export default {
		name: 'game',

		components: {
			HalfLayout,
			Header,
			Footer,
			GameOver,
			AnswerVariant,
			Result,
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
				return this.isGameOver
					? this.$utils.loadAsset(`result-${this.getResult}.jpg`)
					: this.$utils.loadAsset(`question-${this.step}.jpg`);
			},
			getPictureWEBP() {
				return this.isGameOver
					? this.$utils.loadAsset(`result-${this.getResult}.webp`)
					: this.$utils.loadAsset(`question-${this.step}.webp`);
			},
			getMirrorPNG() {
				const emoji = this.currentAnswer ? this.currentAnswer.isCorrect : 'question';

				return this.$utils.loadAsset(`mirror-${emoji.toString()}.png`);
			},
			getMirrorWEBP() {
				const emoji = this.currentAnswer ? this.currentAnswer.isCorrect : 'question';

				return this.$utils.loadAsset(`mirror-${emoji.toString()}.webp`);
			},
			isGameOver() {
				return this.step > Object.keys(this.info.main.info).length;
			},
			getResult() {
				if (this.countCorrect <= 2) {
					return 'low';
				} else if (this.countCorrect > 2 && this.countCorrect <= 4) {
					return 'middle';
				}

				return 'top';
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
			onRepeatClick() {
				this.step = 1;
				this.countCorrect = 0;
				this.currentAnswer = null;
				this.isQuestion = true;
			},
		},
	};
	</script>

	<style lang="scss" scoped>
		.page-game{
			flex-grow: 1;
			position: relative;
			background-color: $blue;

			@include flex(flex, $dir: column);

			&--header {
				position: absolute;
				top: 0;
				left: 0;
				right: 0;
				z-index: 100;
			}

			&--footer {
				@include breakpoint('md') {
					position: absolute;
					bottom: 11px;
				}
			}

			&__team {
				margin: rem(35) auto 0 auto;
				color: $light-blue;
				text-decoration: 1px solid $light-blue;
				text-decoration-line: underline;
				background-color: $blue;

				@include fs(12, 15, 700);

				@include hover() {
					cursor: pointer;
					color: $white;
				}

				@include breakpoint('md') {
					position: absolute;
					left: 59%;
					bottom: rem(19);
					z-index: 11;
					padding-top: 0;
					background-color: transparent;
				}

				&:hover ~ .page-game__popup {
					display: block;
					position: absolute;
					width: rem(262);
					height: rem(193);

					bottom: rem(90);
					left: 50%;
					transform: translateX(-50%);

					@include breakpoint('sm') {
						bottom: rem(80);
					}

					@include breakpoint('md') {
						bottom: rem(30);
						transform: translateX(0);
					}
				}
			}

			&__popup {
				display: none;
			}
		}

		.main {
			flex-grow: 1;
			@include flex(flex, center, flex-start, column);
			position: relative;

			&__mirror {
				position: absolute;
				top: 0;
				left: 50%;
				transform: translateX(-50%);
				width: rem(128);
				height: rem(51);
				z-index: 10;

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
					z-index: 10;
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
					width: rem(456);
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
		}
	</style>
