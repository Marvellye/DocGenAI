```
```vue
<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  fileUrl: string
}>()

const emit = defineEmits(['close'])

const isPdf = computed(() => {
  return props.fileUrl.toLowerCase().endsWith('.pdf');
});

const downloadFile = () => {
  const link = document.createElement('a');
  link.href = props.fileUrl;
  link.download = props.fileUrl.substring(props.fileUrl.lastIndexOf('/') + 1);
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  emit('close');
};
</script>

<template>
  <div class="fixed inset-0 bg-black/80 backdrop-blur-sm flex items-center justify-center p-4 z-50">
    <div class="bg-black/80 border border-green-900/50 rounded-xl w-full max-w-4xl h-[80vh] flex flex-col">
      <div class="p-4 flex justify-between items-center border-b border-green-900/50">
        <h3 class="text-xl font-semibold">Document Preview</h3>
        <button
          @click="$emit('close')"
          class="p-2 hover:bg-green-900/20 rounded-lg transition-colors"
        >
          ✕
        </button>
      </div>
      <div class="flex-1 p-4" v-if="isPdf">
        <iframe
          :src="`https://docs.google.com/gview?url=${encodeURIComponent(fileUrl)}&embedded=true`"
          class="w-full h-full rounded-lg bg-white"
        ></iframe>
      </div>
      <div v-else class="flex-1 p-4 flex items-center justify-center">
        <button @click="downloadFile" class="px-6 py-3 bg-green-500 text-black font-medium rounded-lg hover:bg-green-400 transition-colors">
          Download Document
        </button>
      </div>
      <div class="p-4 flex justify-end" v-if="isPdf">
        <button @click="downloadFile" class="px-6 py-3 bg-green-500 text-black font-medium rounded-lg hover:bg-green-400 transition-colors">
          Download Document
        </button>
      </div>
    </div>
  </div>
</template>
```
```html
