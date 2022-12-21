<template lang="pug">
  .half-layout
    //- Uncomment if need this section
      .half-layout__top
        slot(name="top")
    .half-layout__content
      .half-layout__content-side.half-layout__content-side--left
        slot(name="left")
      .half-layout__content-side.half-layout__content-side--right
        slot(name="right")
    //- Uncomment if need this section
      .half-layout__bottom
        slot(name="bottom")
</template>

<script>
export default {
  name: 'HalfLayout',
};
</script>

<style lang="scss" scoped>
  .half-layout {
    @include flex(flex, $dir: column);
    flex: 1 0 auto;
    position: relative;
    width: 100%;

    &__top {
      flex-shrink: 0;
    }

    &__content {
      @include flex(flex, $dir: column);
      flex: 1 0 auto;
      position: relative;

      @include breakpoint(md) {
        flex-direction: row;
      }
    }

    &__content-side {
      display: flex;
      flex-direction: column;
      flex: 0 0 auto;

      @include breakpoint(md) {
        width: 50%;
        flex: 1 0;
      }

      &--left {
        position: relative;
        overflow: hidden;
        border-bottom: 2px solid $white;

        @include breakpoint(md) {
          min-height: 100vh;
          min-height: auto;
          border-bottom: none;
          border-right: 2px solid $white;
        }
      }

      &--right {
        @include breakpoint(md) {
          height: 100vh;
          overflow: scroll;
        }

        @include breakpoint(xl) {
          position: relative;
          z-index: 100;
        }
      }
    }

    &__bottom {
      flex-shrink: 0;
    }
  }
</style>
