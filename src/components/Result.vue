<template lang="pug">
.result
	p.result__result(qa_quiz_result) {{currentAnswer.result}}

	p.result__extra(v-if="currentAnswer.extra" v-html="currentAnswer.extra")

	Button.result__next(:info="info.main.link" @click="onNextClick" qa_quiz_next)
</template>

<script>
import Button from '@/core/Button.vue';

export default {
	name: 'answer-variant',

	props: {
		info: {
			type: Object,
			required: true,
		},
		currentAnswer: {
			type: Object,
			required: true,
		},
	},

	components: {
		Button,
	},

	methods: {
		onNextClick() {
			this.$emit('next-click');
		},
	},
};
</script>

<style lang="scss" scoped>
	.result {
		&__result {
			margin: rem(16) rem(16) rem(24) rem(16);
			color: $white;
			
			@include fs(18, 26, 700);

			@include breakpoint('md') {
				margin-top: rem(85);
				margin-left: 0;
				margin-right: 0;
			}
		}

		&__extra {
			margin: 0 rem(16) 0 rem(16);
			font-weight: 400;
			color: $white;

			@include breakpoint('md') {
				margin: 0;
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

			@include fs(16, 24, 700);

			@include breakpoint('md') {
				margin-left: 0;
				margin-right: auto;
			}
		}
	}
</style>
