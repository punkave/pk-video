<script>
import { defineComponent } from 'vue';
import PkVideo from '@/pk-video.vue';

export default defineComponent({
  name: 'ServeDev',
  components: {
    PkVideo
  }
});
</script>

<template>
  <div id="app">
    <pk-video />
  </div>
</template>
