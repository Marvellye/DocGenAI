```
```vue
<script setup lang="ts">
import { ref } from 'vue'
import DocumentForm from './components/DocumentForm.vue'
import PreviewModal from './components/PreviewModal.vue'
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'

const showPreview = ref(false)
const generatedFileUrl = ref('')

const handleDocumentGenerated = (url: string) => {
  generatedFileUrl.value = url
  showPreview.value = true
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-black via-green-950 to-black text-white">
    <Navbar />

    <div class="container mx-auto px-4 py-12 pt-32">
      <header class="text-center mb-16">
        <h1 class="text-4xl md:text-6xl font-bold mb-4">Your Free AI <span class="text-green-400">PDF/PPTX</span> <br/> Generator is Here!</h1>
        <p class="text-gray-400 text-lg md:text-xl max-w-2xl mx-auto">
          Generate professional research documents, papers, and presentations effortlessly with AI.
          <span class="text-green-400">Currently in beta testing phase.</span>
        </p>
      </header>

      <DocumentForm @document-generated="handleDocumentGenerated" />

      <PreviewModal v-if="showPreview" :file-url="generatedFileUrl" @close="showPreview = false" />
    </div>
    <Footer />
  </div>
</template>
```
```html
