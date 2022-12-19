<template lang="pug">
//- Default
//-   main.main
//-     HalfLayout
//-       template(v-slot:left)
//-         .main__preview
//-       template(v-slot:right)
//-         .main__info-card.info-card
//-           .info-card__title-container
//-             .container
//-               h1.info-card__title
//-                 SvgEl(:name="info.main.title.svg")

//-           .info-card__info-container
//-             p.info-card__info(v-html="info.main.info")

//-             Button.info-card__begin-test(:info="info.main.link")

Default
  main.main
    .main__preview

    .main__info-card.info-card
      .info-card__title-container
        .container
          h1.info-card__title
            SvgEl(:name="info.main.title.svg")

      .info-card__info-container
        p.info-card__info(v-html="info.main.info")

        Button.info-card__begin-test(:info="info.main.link")
</template>

<script>
// import Example from '@/components/Example.vue';
import Default from '@/layouts/Default.vue';
import HalfLayout from '@/layouts/HalfLayout.vue';
import SvgEl from '@/core/SvgEl.vue';
import Button from '@/core/Button.vue';

export default {
  name: 'index',
  components: {
    // Example,
    Default,
    HalfLayout,
    SvgEl,
    Button,
  },
  data: (context) => ({
    info: context.$utils.loadJSON('indexPage.json'),
  }),
};
</script>

<style lang="scss" scoped>
  .main {
    flex-grow: 1;
    @include flex(flex, center, flex-start, column);
    background-color: $dark-blue;
    position: relative;

    &__preview {
      width: 100%;
      height: rem(249);
      background: url("../assets/images/bg-preview__start--mobile.jpg") no-repeat 50% 0;
      background-size: cover;

      @media (min-resolution: 144dpi),
        (min-resolution: 1.5dppx) {
        background: url("../assets/images/bg-preview__start--mobile@2x.jpg") no-repeat 50% 0;
        background-size: cover;
      }

      @include breakpoint('sm') {
        background: url("../assets/images/bg-preview__start--desktop.jpg") no-repeat 50% 0;
        background-size: cover;

        @media (min-resolution: 144dpi),
          (min-resolution: 1.5dppx) {
          background: url("../assets/images/bg-preview__start--desktop@2x.jpg") no-repeat 50% 0;
          background-size: cover;
        }
      }

      @include breakpoint('md') {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        height: 100vh;
        width: 100vw;
      }
    }

    &__info-card {
      width: 100%;
      flex-grow: 1;
      border-top: 2px solid $white;
      border-bottom: 2px solid $white;

      @include breakpoint('md') {
        position: absolute;
        top: rem(72);
        right: 0;
        bottom: 0;
        height: rem(480);
        width: 50%;
        background-color: $dark-blue;
        border: 2px solid $white;
      }
    }
  }

  .info-card {
    &__title {
      padding: rem(24) 0;

      svg {
        margin: 0 auto;
        width: rem(288);
        height: rem(62);

        @include breakpoint('md') {
          width: rem(428);
          height: rem(92);
        }
      }
    }

    &__title-container {
      border-bottom: 2px solid $white;

      .container {
        @include breakpoint('md') {
          width: auto;
        }
      }
    }

    &__info-container {
      margin-top: rem(6);
      padding: rem(32) rem(16) rem(70) rem(16);
      border-top: 2px solid $white;

      @include breakpoint('sm') {
        padding-left: rem(65);
        padding-right: rem(65);
      }

      @include breakpoint('md') {
        padding-left: rem(96);
        padding-right: rem(108);
      }
    }

    &__info {
      margin-bottom: rem(40);
      color: $white;

      ::v-deep .link {
        color: $light-blue;
        text-decoration: 1px solid $light-blue;
        text-decoration-line: underline;
        font-weight: 700;
      }
    }

    &__begin-test {
      display: block;
      margin: 0 auto;

      @include breakpoint('md') {
        margin-left: 0;
        margin-right: auto;
        width: rem(250);
      }
    }
  }
</style>
