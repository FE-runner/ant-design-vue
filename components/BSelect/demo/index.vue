<template>
  <demo-sort :cols="1">
    <Basic />
  </demo-sort>
</template>
<script lang="ts">
import Basic from './basic.vue';
import CN from '../index.zh-CN.md';
import { defineComponent } from 'vue';

export default defineComponent({
  CN,
  components: {
    Basic,
  },
  setup() {
    return {};
  },
});
</script>
