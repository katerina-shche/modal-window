<template>
    <ModalBase/>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

import ModalBase from '@/components/ModalBase.vue';

export default defineComponent({
  name: 'App',
  components: {
    ModalBase
  },
  setup() {
    return {};
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
