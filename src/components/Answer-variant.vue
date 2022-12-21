<template lang="pug">
.answer-variant
	h1.answer-variant__title
		SvgEl(:name="info.main.title.svg")

	ul.answer-variant__answer-list
		li.answer-variant__answer-item(v-for="(answer, idx) of info.main.info['step-' + step]" :key="idx" @click="onAnswerClick(answer)" v-html="answer.answer")

</template>
	
<script>
import SvgEl from '@/core/SvgEl.vue';

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
	}
</style>
