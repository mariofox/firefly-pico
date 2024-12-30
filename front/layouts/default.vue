<template>
  <div :class="layoutClass">
    <!--    TODO: Implement me later...   -->
    <template v-if="device.isMobile || true">
      <slot />
      <app-bottom-toolbar />
    </template>

    <template v-else>
      <app-left-sidebar />
      <slot />
    </template>

    <app-bottom-loading />
  </div>
</template>

import { ref } from 'vue';

<script setup>
const device = useDevice()

const layoutClass = computed(() => {
  return {
    layout: true,
    desktop: device.isDesktop,
    mobile: device.isMobile,
  }
})
</script>
