<template lang="pug">
.default
  Header.default__header(qa_header)
  .default__main
    slot
  Footer.default__footer(qa_footer)
</template>

<script>
import Header from '@/components/Header/Header.vue';
import Footer from '@/components/Footer.vue';

export default {
  name: 'Default',
  components: {
    Header,
    Footer,
  },
};
</script>

<style scoped lang="scss">
  .default {
    @include flex(flex, $dir: column);
    flex: 1 0 auto;
    // Padding for fixed header height to keep it always top
    // padding-top: rem(70);

    &__header {
      // Recommend to use position fixed for cross-browser compatibility
      // Sticky could be a bit buggy in Edge or Safari
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      z-index: 100;
    }

    &__main {
      @include flex(flex, $dir: column);
      flex: 1 0 auto;
    }
  }
</style>
