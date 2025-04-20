<template>
  <div class="container">
    <h1 class="text-4xl font-bold mb-6 text-center text-blue-700">🖋️ wordCounter</h1>

    <textarea
      v-model="text"
      placeholder="Escribe tu texto aquí..."
      class="w-full h-72 p-5 border-2 border-blue-500 rounded-2xl shadow-lg resize-none focus:outline-none focus:ring-4 focus:ring-blue-300 transition-all"
    ></textarea>

    <div class="mt-6 text-center text-blue-800">
      <p class="text-xl mb-2"><strong class="text-blue-600">Palabras:</strong> {{ wordCount }}</p>
      <p class="text-xl"><strong class="text-blue-600">Caracteres:</strong> {{ charCount }}</p>
    </div>

    <div class="mt-8 flex justify-center">
      <button
        @click="clearText"
        class="px-8 py-3 bg-gradient-to-r from-blue-400 to-indigo-600 text-white font-bold rounded-2xl shadow-xl hover:from-blue-500 hover:to-indigo-700 transition-all duration-300"
      >
        Borrar Todo
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const text = ref('')

// Computed properties to calculate word and character count
const wordCount = computed(() => {
  return text.value.trim() === ''
    ? 0
    : text.value.trim().split(/\s+/).length
})

const charCount = computed(() => text.value.length)

// Method to clear the textarea
const clearText = () => {
  text.value = ''
}
</script>

<style scoped>
/* Estilos del contenedor */
.container {
  max-width: 700px;
  margin: 40px auto;
  padding: 30px;
  background-color: #ffffff;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #e0f2fe 0%, #bbdefb 100%);
}

/* Estilos del textarea */
textarea {
  font-size: 18px;
  line-height: 1.6;
  background: #f0f9ff;
  color: #333;
  transition: all 0.3s ease;
}

textarea:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 8px rgba(59, 130, 246, 0.4);
}

/* Estilos del botón */
button {
  transition: all 0.3s ease;
}

button:hover {
  background-color: #3b82f6;
}
</style>
