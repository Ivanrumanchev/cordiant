<template lang="pug">
.answer-variant
	h1.answer-variant__title
		SvgEl(:name="info.main.title.svg")

	ul.answer-variant__answer-list
		li.answer-variant__answer-item(v-for="(answer, idx) of info.main.info['step-' + step]" :key="idx")
			Button.answer-variant__button(@click.prevent="onAnswerClick(answer)" :info="{ text: answer.answer }" qa_quiz_answer)
</template>
	
<script>
import SvgEl from '@/core/SvgEl.vue';
import Button from '@/core/Button.vue';

export default {
	name: 'answer-variant',

	props: {
		info: {
			type: Object,
			required: true,
		},
		step: {
			type: Number,
			required: true,
		},
	},

	components: {
		Button,
		SvgEl,
	},

	methods: {
		onAnswerClick(answer) {
			this.$emit('answer-click', answer);
		},
	},
};
</script>

<style lang="scss" scoped>
	.answer-variant {
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

			&:not(:last-child) {
				margin-bottom: rem(16);
			}
		}

		&__button {
			display: block;
			width: 100%;
			height: 100%;
			min-height: rem(82);
			padding: rem(20) rem(8) rem(20) rem(16);
			border-radius: 3px;
			background-color: $answer-color;
			color: $white;
			text-align: start;

			@include fs(16, 24, 400);

			@include breakpoint('md') {
				padding: rem(16) rem(38) rem(18) rem(48);
			}

			&:hover {
				background-color: $light-blue;
				color: $dark-blue-text;
				transition: background-color .3s ease-in;
				cursor: pointer;
			}

			&:active {
				background-color: $white;
				color: $dark-blue-text;
			}
		}
	}
</style>
