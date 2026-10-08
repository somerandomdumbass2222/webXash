<template>
  <XashSettings v-if="showXashSettingUI" />
  <XashCanvas />
  <XashInGameSaves v-if="!showXashSettingUI" />
</template>

<script setup lang="ts">
  import { storeToRefs } from 'pinia';
  import { useXashStore } from '/@/stores/store.ts';
  import XashSettings from '/@/components/XashSettings.vue';
  import XashCanvas from '/@/components/XashCanvas.vue';
  import XashInGameSaves from '/@/components/XashInGameSaves.vue';

  const store = useXashStore();
  const { showXashSettingUI } = storeToRefs(store);
</script>

<style scoped lang="scss">
  .start {
    position: absolute;
    bottom: 10px;
    right: 10px;
  }
</style>
