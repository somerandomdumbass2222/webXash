<template>
  <div class="save-transfer">
    <button class="save-transfer__btn" @click="exportAll">Export all saves (.zip)</button>
    <button class="save-transfer__btn" @click="importSaves">
      Import saves (.sav / .zip)
    </button>
    <span v-if="message" class="save-transfer__msg">{{ message }}</span>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue';
  import { SaveManager } from '/@/services';
  import { useXashStore } from '/@/stores/store';

  const store = useXashStore();
  const message = ref('');

  const say = (text: string) => {
    message.value = text;
    setTimeout(() => (message.value = ''), 4000);
  };

  const exportAll = async () => {
    const zip = await SaveManager.exportAllAsZip();
    if (!zip) return say('No saves found yet.');
    const url = URL.createObjectURL(
      new Blob([zip as BlobPart], { type: 'application/zip' }),
    );
    const a = document.createElement('a');
    a.href = url;
    a.download = 'webxash-saves.zip';
    document.body.appendChild(a);
    a.click();
    a.remove();
    URL.revokeObjectURL(url);
    say('Exported.');
  };

  const importSaves = () => {
    const input = document.createElement('input');
    input.type = 'file';
    input.multiple = true;
    input.accept = '.sav,.zip';
    input.onchange = async () => {
      const files = Array.from(input.files ?? []);
      if (!files.length) return;
      try {
        const n = await SaveManager.importFiles(files);
        await store.refreshSavesList();
        say(n ? `Imported ${n} save(s).` : 'No .sav files found.');
      } catch (e) {
        console.error(e);
        say('Import failed (see console).');
      }
    };
    input.click();
  };
</script>

<style scoped lang="scss">
  .save-transfer {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    align-items: center;
    margin-bottom: 0.75rem;
  }
  .save-transfer__msg {
    font-size: 0.85rem;
  }
</style>
