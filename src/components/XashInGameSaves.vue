<template>
  <div class="ingame-saves">
    <button class="ingame-saves__toggle" @click="open = !open">
      {{ open ? 'Close' : 'Saves' }}
    </button>
    <div v-if="open" class="window ingame-saves__panel">
      <p>
        Export grabs everything the game has saved so far. After importing, the
        saves appear in the in-game Load Game menu.
      </p>
      <XashSaveTransfer />
    </div>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue';
  import XashSaveTransfer from './XashSaveTransfer.vue';
  const open = ref(false);
</script>

<style scoped lang="scss">
  .ingame-saves {
    position: fixed;
    top: 8px;
    right: 8px;
    z-index: 50;
    opacity: 0.35;
    &:hover {
      opacity: 1;
    }
    &__panel {
      margin-top: 4px;
      max-width: 22rem;
    }
  }
</style>
