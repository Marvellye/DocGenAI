<script setup lang="ts">
defineProps<{
  fileUrl: string
}>()

defineEmits(['close'])
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
      <div class="flex-1 p-4">
        <iframe
          :src="`https://docs.google.com/gview?url=${encodeURIComponent(fileUrl)}&embedded=true`"
          class="w-full h-full rounded-lg bg-white"
        ></iframe>
      </div>
    </div>
  </div>
</template>