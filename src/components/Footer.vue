<template lang="pug">
footer.footer(qa_footer)
  .container.footer__container
    Share(v-if="!withoutShare").footer__share(:theme="'blue'", rounded)

    .footer__copyright
      a(
        :href="info.main.url"
        target="_blank"
        rel="noopener"
        v-html="info.main.text"
        ga_logo_lh_footer)
      | |&nbsp;
      a(v-if="info.partner" :href="info.partner.url" target="_blank" rel="noopener"
        v-html="info.partner.text" qa_logo_partner_footer qa_link_external)
      span © {{ getCurrentYear() }}
</template>

<script>
import Share from '@/core/Share.vue';

export default {
  name: 'Footer',
  props: {
    withoutShare: {
      type: Boolean,
      default: false,
    },
  },
  data: (context) => ({
    info: context.$utils.loadJSON('template/common.json'),
  }),
  components: {
    Share,
  },
  methods: {
    getCurrentYear() {
      return new Date().getFullYear();
    },
  },
};
</script>

<style scoped lang="scss">
.footer {
  background-color: $blue;
  padding-bottom: rem(26);
  width: 100%;

  @include breakpoint('md') {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: transparent;
  }

  @include breakpoint(sm) {
    padding: 0;
    height: rem(66);
  }

  &__container {
    text-align: center;
    height: 100%;
    @include flex(flex, center, space-between, $dir: column);

    @include breakpoint(sm) {
      @include flex(flex, center, space-between, $dir: row);
      height: 100%;
      text-align: left;
    }
  }

  &__share {
    margin-top: rem(40);

    @include breakpoint(sm) {
      justify-content: flex-start;
      margin-top: 0;
    }
  }

  &__copyright {
    margin-top: rem(40);

    @include fs(12, 15, 700);
    @include flex(inline-flex, center);
    color: $light-blue;

    @include breakpoint(sm) {
      align-self: flex-end;
      margin-top: 0;
      margin-bottom: 8px;
      margin-left: auto;
    }

    a {
      color: inherit;
      margin-right: 5px;
      text-decoration: 1px solid $light-blue;
      text-decoration-line: underline;

      &:not(:first-of-type) {
        @include breakpoint(sm) {
          margin-left: 5px;
        }
      }

      @include hover(true) {
        color: $white;
      }

      @include breakpoint(lg) {
        transition: color $ease;
      }
    }

    span {
      line-height: rem(15);
      font-weight: 400;
      color: $white;
    }
  }
}
</style>
