<template lang="pug">
div(id="app")
  //- Default ayout usage example
  //- Default
    //- specify directly if have only one page(route)
    Index
    //- if multiple routes (uncomment router in main.js)
        and next line
    //- router-view
  //- Half-layout usage example
  HalfExample
</template>

<script>
// Import layout
import Default from '@/layouts/Default.vue';

import Index from '@/pages/Index.vue';

import HalfExample from '@/pages/HalfExample.vue';

export default {
  components: {
    HalfExample,
    Default,
    Index,
  },
};
</script>
