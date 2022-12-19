<script>
export default {
  name: 'Button',
  props: {
    info: {
      required: true,
      default: false,
      validator(value) {
        return typeof value === 'object' || typeof value === 'boolean';
      },
    },
  },
  render(createElement) {
    let elProps = {
      class: 'button',
      domProps: {
        innerHTML: this.info?.text,
      },
      on: this.$listeners,
    };
    if (this.info.url) {
      elProps = {
        ...elProps,
        attrs: {
          href: this.info.url,
          target: this.info.external ? '_blank' : 'self',
          rel: 'noopener',
        },
      };
    }
    return createElement(this.info.url ? 'a' : 'button', elProps);
  },
};
</script>

<style scoped lang="scss">
.button {
  position: relative;
  //- Just example styles to set all buttons visible
  height: rem(56);
  min-width: rem(20);
  padding: 16px;
  background-color: $light-blue;
  color: $dark-blue-text;
  text-align: center;
  font-weight: 700;
  box-shadow: 0px 4px 52px rgba(0, 39, 66, 0.9);
  border-radius: 3px;

  @include hover {
    background-color: $white;
  }
}
</style>
