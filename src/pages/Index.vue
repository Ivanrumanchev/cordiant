<template lang="pug">
//- Example(v-if="info.example" :info="info.example")
Default
</template>

<script>
// import Example from '@/components/Example.vue';
import Default from '@/layouts/Default.vue';

export default {
  name: 'index',
  components: {
    // Example,
    Default,
  },
  data: (context) => ({
    info: context.$utils.loadJSON('indexPage.json'),
  }),
};
</script>
