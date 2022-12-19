<template lang="pug">
  section.test
    .test__container.test-preview(v-if="!isTest && !isResult")
      .test-preview__title(v-html="test.title")
      .test-preview__description(v-html="test.description")
      .button(@click="showTest" v-html="test.startButtonName")

    .test__container.test-questions(v-if="isTest")
      .test-questions__question(
        v-html="test.questions[currentQuestionIndex].text")
      .test-questions__answers
        .test-questions__answer(
          v-for="(answer, index) in test.questions[currentQuestionIndex].answers"
          :key="index"
          @click="selectAnswer($event, index, answer.id)"
          v-html="answer.text")

    .test__container.test-result(v-if="isResult")
      .test-result__title(v-html="test.results[currentResultIndex].title")
      .test-result__text(v-html="test.results[currentResultIndex].text")
      .test-result__toolbar
        .test__share
          .share-label Поделиться
          Share
        button.button(@click="restartTest") Пройти еще раз
</template>

<script>
import Share from '@/core/Share.vue';

export default {
  name: 'Test',
  components: {
    Share,
  },
  data: (context) => ({
    test: context.$utils.loadJSON('template/test.json'),
    isTest: false,
    isResult: false,
    currentQuestionIndex: 0,
    currentResultIndex: null,
  }),
  methods: {
    showTest() {
      this.isTest = true;
      window.scrollTo(0, 0);
    },
    restartTest() {
      this.isResult = false;
      this.isTest = true;
      this.currentQuestionIndex = 0;
      this.currentResultIndex = null;
      if (this.test.withRightAnswer) {
        this.rightAnswersCounter = 0;
      } else {
        this.resultsCounter = new Array(this.answersAmount).fill(0);
      }
      window.scrollTo(0, 0);
    },
    selectAnswer(e, selectedAnswerIndex, selectedAnswerId) {
      if (this.currentQuestionIndex < this.test.questions.length - 1) {
        this.currentQuestionIndex += 1;

        if (this.test.withRightAnswer) {
          const { rightAnswerId } = this.test.questions[this.currentQuestionIndex];
          if (rightAnswerId === selectedAnswerId) this.rightAnswersCounter += 1;
        } else {
          this.resultsCounter[selectedAnswerIndex] += 1;
        }
      } else {
        if (this.test.withRightAnswer) {
          // eslint-disable-next-line max-len
          this.currentResultIndex = this.rightAnswersCounter === 0 ? 0 : this.rightAnswersCounter - 1;
        } else {
          const max = Math.max.apply(null, this.resultsCounter);
          this.currentResultIndex = this.resultsCounter.indexOf(max);
        }

        this.isTest = false;
        this.isResult = true;
      }
      window.scrollTo(0, 0);
    },
  },
  created() {
    if (this.test.withRightAnswer) {
      this.rightAnswersCounter = 0;
    } else {
      this.answersAmount = 5;
      this.resultsCounter = new Array(this.answersAmount).fill(0);
    }
  },
};
</script>

<style lang="scss">
  .test {
    display: flex;
    flex-direction: column;
    scroll-behavior: smooth;

    &__container {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      position: relative;
    }

    .test-questions {

      &__question {
      }

      &__answers {
        display: flex;
        flex-direction: column;
      }
      &__answer {
        cursor: pointer;

        &:not(:last-child) {
          margin-bottom: 20px;
        }
      }
    }

    .test-result {

      &__title {
      }
      &__text {
      }
      &__toolbar {
        display: flex;
        flex-direction: column;
        align-items: center;
        position: relative;
        align-self: center;
        @include breakpoint(sm) {
          flex-direction: row;
          align-self: unset;
        }
      }
    }
    &__share {
      display: flex;
      flex-direction: column;
      align-items: center;
      @include breakpoint(sm) {
        align-items: unset;
      }
    }

    .button {
      color: $white;
      min-width: rem(20);
      background-color: $lifehacker;
      padding: 5px;
    }
  }
</style>
