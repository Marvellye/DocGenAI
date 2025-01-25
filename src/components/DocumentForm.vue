```
```vue
<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['document-generated'])

const prompt = ref('')
const documentType = ref('pdf')
const isLoading = ref(false)
const email = ref('')
const emailLoading = ref(false)

const handleSubmit = async () => {
  if (!prompt.value) return

  isLoading.value = true
  try {
    const medium = documentType.value
    const response = await fetch(
      `https://api.marvelly.com.ng/v1/${medium}?text=${encodeURIComponent(prompt.value)}`
    )

    if (!response.ok) {
      const errorData = await response.json();
      const errorMessage = errorData.message || `HTTP error! status: ${response.status}`;
      throw new Error(errorMessage);
    }

    const data = await response.json()

    if (data.status === 'success' || data.message.includes('successfully')) {
      const fileUrl = data.pdf_path || data.file_url
      emit('document-generated', fileUrl)
    } else {
      throw new Error('Generation failed')
    }
  } catch (error) {
    console.error('Error generating document:', error)
    alert(`Failed to generate document: ${error}`)
  } finally {
    isLoading.value = false
  }
}

const handleEmailSubmit = async () => {
  if (!email.value) return;

  emailLoading.value = true;
	
try {
  const response = await fetch(
    `https://api.marvelly.com.ng/v1/news-letter?service=DocGenAI&email=${encodeURIComponent(email.value)}`
  );

  if (!response.ok) {
    const errorData = await response.json();
    const errorMessage = errorData.message || `HTTP error! Status: ${response.status}`;
    throw new Error(errorMessage);
  }

  const data = await response.json();

  if (data.success === 'true') {
    alert('Subscribed successfully!');
    email.value = '';
  } else {
    throw new Error(data.error || 'Subscription failed');
  }
} catch (error) {
  console.error('Error subscribing:', error.message);
  
  // Handle error response properly
  alert(`Failed to subscribe: ${error.message || 'Unknown error occurred'}`);
} finally {
  emailLoading.value = false;
}
}
</script>

<template>
  <div class="max-w-2xl mx-auto bg-black/50 backdrop-blur-lg rounded-xl p-6 shadow-xl border border-green-900/50">
    <form @submit.prevent="handleSubmit" class="space-y-6">
      <div>
        <label class="block text-sm font-medium mb-2">Document Type</label>
        <select v-model="documentType" class="w-full px-4 py-2 rounded-lg bg-black/50 border border-green-900 focus:outline-none focus:border-green-500 transition-colors text-white">
          <option value="pdf">PDF (Stable)</option>
          <option value="pptx">PPTX (Unstable)</option>
        </select>
      </div>

      <div>
        <label class="block text-sm font-medium mb-2">Your Prompt</label>
        <textarea v-model="prompt" rows="4" required class="w-full px-4 py-2 rounded-lg bg-black/50 border border-green-900 focus:outline-none focus:border-green-500 transition-colors text-white" placeholder="Describe the document you want to create..."></textarea>
      </div>

      <button type="submit" :disabled="isLoading || !prompt" class="w-full px-6 py-3 bg-green-500 text-black font-medium rounded-lg hover:bg-green-400 transition-colors disabled:opacity-50 disabled:cursor-not-allowed relative">
        <span v-if="!isLoading">Generate Document</span>
        <div v-else class="loading-spinner"></div>
      </button>
    </form>
  </div>
 <div class="mt-16 max-w-2xl mx-auto bg-black/50 backdrop-blur-lg rounded-xl p-6 shadow-xl border border-green-900/50">
		<div class="text-center">
      <h2 class="text-2xl font-semibold mb-4">Stay Updated</h2>
      <p class="text-gray-400 mb-6">Join our newsletter to get early access and latest updates</p>
      <form @submit.prevent="handleEmailSubmit" class="max-w-md mx-auto">
        <div class="flex flex-col sm:flex-row gap-2">
          <input type="email" v-model="email" placeholder="Enter your email" class="flex-1 px-4 py-2 rounded-lg bg-black/50 border border-green-900 focus:outline-none focus:border-green-500 transition-colors text-white">
          <button type="submit" :disabled="emailLoading" class="px-6 py-2 bg-green-500 text-black font-medium rounded-lg hover:bg-green-400 transition-colors relative w-full sm:w-auto">
            <span v-if="!emailLoading">Subscribe</span>
            <div v-else class="loading-spinner"></div>
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
```
```html
