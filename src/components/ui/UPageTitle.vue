<template>
  <component :is="'h' + props.level" class="u-page-title">
    <slot name="default"></slot>
  </component>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  level: {
    type: [Number, String],
    default: 1
  }
})
</script>

<style lang="scss" scoped>
.u-page-title {
  font-size: rem(19);
  line-height: rem(27);
  font-weight: 700;
  letter-spacing: -0.0035em;
}
</style>